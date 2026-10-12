# Completion

How this OTP completes. \"code\": the user enters the code and you call POST /otp/verify. \"inbound\": the user opens action_url and sends the prefilled WhatsApp message; that message approves the OTP, no code is sent. Poll GET /otp/:id or take the otp.approved webhook.

## Enum

* `CODE` (value: `'code'`)

* `INBOUND` (value: `'inbound'`)

[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


