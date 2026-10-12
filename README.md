# otp.com Python SDK

Python client for the [otp.com](https://otp.com) OTP API: send a one-time password, verify the code
the user entered, resend it on another channel.

Requires Python 3.10+. Fully type-annotated, models are Pydantic v2.

- **API contract:** [otp-com/sdk](https://github.com/otp-com/sdk) (`openapi.yaml`)
- **Method and model reference:** [`docs/`](./docs)
- **Other languages:** [Node.js](https://github.com/otp-com/sdk-node) ·
  [PHP](https://github.com/otp-com/sdk-php) · [Go](https://github.com/otp-com/sdk-go) ·
  [MCP server](https://github.com/otp-com/mcp)

## Install

Not on PyPI yet, so install from GitHub:

```sh
pip install "otp-sdk @ git+https://github.com/otp-com/sdk-python@v1.2.0"
```

The same line works in `requirements.txt`. Swap the tag for `main` to follow the branch, or for a
commit SHA to pin harder than a tag:

```
otp-sdk @ git+https://github.com/otp-com/sdk-python@<commit-sha>
```

uv and Poetry take the same source:

```sh
uv add "otp-sdk @ git+https://github.com/otp-com/sdk-python@v1.2.0"
poetry add "git+https://github.com/otp-com/sdk-python#v1.2.0"
```

The distribution is `otp-sdk`, the import is `otp_sdk`. Once the package is on PyPI, `pip install
otp-sdk` is enough.

## Quickstart

Get an API key from the otp.com panel under **API Keys**. `otp_live_…` sends for real, `otp_test_…`
runs in sandbox. Keep it server-side; it is a bearer credential.

```python
import os
import otp_sdk

config = otp_sdk.Configuration(access_token=os.environ["OTP_API_KEY"])

with otp_sdk.ApiClient(config) as client:
    otp = otp_sdk.OTPApi(client)

    # 1. Send. You pass the recipient; your account routing picks the channel.
    # client_ip = the END USER's IP from your request context, not your server's:
    # requests without it share a much tighter rate limit.
    sent = otp.send_otp(otp_sdk.SendRequest(recipient="+14155552671", locale="en", client_ip="81.2.69.142"))

    sent.otp_id            # keep this: you verify against it
    sent.channel           # "sms" | "whatsapp" | "email" | "telegram"
    sent.masked_recipient  # "+14****71", safe to show the user
    sent.action_url        # WhatsApp only, see below
    sent.completion        # "code" | "inbound", see below

    # 2. Verify whatever the user typed in.
    result = otp.verify_otp(otp_sdk.VerifyRequest(otp_id=sent.otp_id, code="123456"))

    if result.matched:
        ...  # The code was correct; result.status is "approved".
```

The code itself is never returned by the API. `recipient` is a phone number in E.164 or an email
address; which one is valid depends on the channels enabled for your app.

### Retries that must not double-send

Pass an idempotency key and a repeat of the same call replays the first response instead of sending
a second code. Reusing a key with a different body is a `409`.

```python
otp.send_otp(
    otp_sdk.SendRequest(recipient="+14155552671"),
    idempotency_key=f"signup:{user_id}",
)
```

## WhatsApp: how the OTP completes

Every send, resend and status response carries `completion`, which tells you how this OTP finishes.
SMS, email and Telegram always complete with `code`. Which WhatsApp mode applies is configured per
account:

| `completion` | `action_url` | What happens |
| --- | --- | --- |
| `code` | `None` | The code was delivered (any channel, or WhatsApp in template mode). The user enters it; you call `verify_otp`. |
| `code` | set | WhatsApp code-back. The user opens the link and sends us the prefilled message; we reply with the code. Then `verify_otp` as on SMS. |
| `inbound` | set | WhatsApp inbound approval. The prefilled message itself approves the OTP; there is no code. Poll `get_otp_status` every 3 to 5 seconds until `status` leaves `pending`, or take the `otp.approved` webhook. |

```python
sent = otp.send_otp(otp_sdk.SendRequest(recipient=recipient))

if sent.action_url:
    # Open it for the user. They send us the prefilled message from their own WhatsApp.
    redirect(sent.action_url)

if sent.completion == otp_sdk.Completion.INBOUND:
    # No code is coming. Wait for that message to approve the OTP.
    current = otp.get_otp_status(sent.otp_id)
else:
    ...  # A code was (or will be) delivered; verify_otp with whatever the user typed in.
```

Calling `verify_otp` on an `inbound` OTP never matches while it is pending and spends an attempt;
once approved it simply reports the approved OTP. Sandbox OTPs always complete with `code`; on
WhatsApp they still return a sample `action_url`, verify with the sandbox code. If the user has no
WhatsApp, resend on a channel they do have: leaving WhatsApp for a code channel sets `completion`
back to `code`.

## Resending

```python
# Advance to the next channel in your routing order.
otp.resend_otp(otp_sdk.ResendRequest(otp_id=sent.otp_id))

# Or move it onto a specific channel, e.g. the user has no WhatsApp.
otp.resend_otp(otp_sdk.ResendRequest(otp_id=sent.otp_id, channel="sms"))
```

A resend before the cooldown elapses is a `429`; a channel that isn't enabled for your app or the
recipient is a `409`.

## Checking status

```python
current = otp.get_otp_status(sent.otp_id)
current.status  # "pending" | "approved" | "failed" | "expired"
```

On `code` OTPs this is for reconciliation and support tooling, not a substitute for `verify_otp`,
which is what actually approves them. On `inbound` OTPs it is how you learn the approval without
the webhook.

## Exchanging a mobile verification token

If your app uses one of the mobile SDKs, the device verifies against the client API and receives a
`verification_token` instead of a result. Exchange it on your backend to learn which recipient was
actually verified; what the app claims about its own verification is not trustworthy.

```python
verifications = otp_sdk.VerificationsApi(client)
verified = verifications.exchange_verification(
    otp_sdk.VerificationExchangeRequest(verification_token=token_from_the_app)
)

verified.recipient  # the number or address the device actually proved
```

## Errors

Any non-2xx response raises `ApiException`. The body is always
`{"error": {"type", "message", "details"?}}`, where `type` is a stable machine-readable class.

```python
import json
from otp_sdk.rest import ApiException

try:
    otp.send_otp(otp_sdk.SendRequest(recipient=recipient))
except ApiException as exc:
    error = json.loads(exc.body)["error"]
    print(exc.status, error["type"], error["message"])
    raise
```

| Status | When |
| --- | --- |
| `401` | Missing or invalid API key, disabled app, or suspended company |
| `404` | Unknown `otp_id` (also returned for another company's OTP, to avoid probing) |
| `409` | No enabled channel, channel not enabled, resend not allowed, or idempotency-key conflict |
| `422` | Request body failed validation |
| `429` | Resend cooldown has not elapsed |

## Configuration

```python
config = otp_sdk.Configuration(
    access_token=os.environ["OTP_API_KEY"],   # required
    host="https://api.otp.com/api/v1",        # default
    retries=3,                                # urllib3 retry policy
)
```

Reuse a single `ApiClient` across requests: it owns the connection pool. Use it as a context
manager, or call `client.close()` when you are done.

## API reference

| Method | Endpoint | Returns |
| --- | --- | --- |
| [`send_otp`](./docs/OTPApi.md#send_otp) | `POST /otp/send` | [`OtpResponse`](./docs/OtpResponse.md) |
| [`verify_otp`](./docs/OTPApi.md#verify_otp) | `POST /otp/verify` | [`VerifyResponse`](./docs/VerifyResponse.md) |
| [`resend_otp`](./docs/OTPApi.md#resend_otp) | `POST /otp/resend` | [`OtpResponse`](./docs/OtpResponse.md) |
| [`get_otp_status`](./docs/OTPApi.md#get_otp_status) | `GET /otp/{otp_id}` | [`OtpStatusResponse`](./docs/OtpStatusResponse.md) |
| [`exchange_verification`](./docs/VerificationsApi.md#exchange_verification) | `POST /verifications/exchange` | [`VerificationExchangeResponse`](./docs/VerificationExchangeResponse.md) |

## Regenerating

Everything in this repo except this README is generated from
[`openapi.yaml`](https://github.com/otp-com/sdk) by
[OpenAPI Generator](https://openapi-generator.tech). Fix the contract, not `otp_sdk/`; a pull
request against generated files will be overwritten by the next regeneration.

- **In CI:** run the **Regenerate from spec** workflow, or let `otp-com/sdk` dispatch it.
- **Locally:** `./update-sdk.sh sdk-python` from a checkout of `otp-com/sdk`.

`README.md` is listed in `.openapi-generator-ignore` so it survives regeneration. When the contract
changes, update it by hand.

## License

MIT
