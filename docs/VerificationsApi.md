# otp_sdk.VerificationsApi

All URIs are relative to *https://api.otp.com*

Method | HTTP request | Description
------------- | ------------- | -------------
[**exchange_verification**](VerificationsApi.md#exchange_verification) | **POST** /api/v1/verifications/exchange | Exchange a verification token for the recipient it proves.


# **exchange_verification**
> VerificationExchangeResponse exchange_verification(verification_exchange_request)

Exchange a verification token for the recipient it proves.

Call this from YOUR backend, with a server key from your API Keys page, using the verification_token your app received from POST /client/otp/verify. It returns the recipient that was actually verified. This is the only trustworthy answer to "did this user prove they control this number": the `matched` field the device saw is a UI hint, read off a device you do not control, and an app can claim anything. Exchanging is idempotent for the same API key within the token lifetime, so a retry after a network failure returns the same result instead of losing the verification. Any other key, a second use, or an expired token gets a 404.

### Example

* Bearer Authentication (bearerAuth):

```python
import otp_sdk
from otp_sdk.models.verification_exchange_request import VerificationExchangeRequest
from otp_sdk.models.verification_exchange_response import VerificationExchangeResponse
from otp_sdk.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to https://api.otp.com
# See configuration.py for a list of all supported configuration parameters.
configuration = otp_sdk.Configuration(
    host = "https://api.otp.com"
)

# The client must configure the authentication and authorization parameters
# in accordance with the API server security policy.
# Examples for each auth method are provided below, use the example that
# satisfies your auth use case.

# Configure Bearer authorization: bearerAuth
configuration = otp_sdk.Configuration(
    access_token = os.environ["BEARER_TOKEN"]
)

# Enter a context with an instance of the API client
with otp_sdk.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = otp_sdk.VerificationsApi(api_client)
    verification_exchange_request = otp_sdk.VerificationExchangeRequest() # VerificationExchangeRequest | 

    try:
        # Exchange a verification token for the recipient it proves.
        api_response = api_instance.exchange_verification(verification_exchange_request)
        print("The response of VerificationsApi->exchange_verification:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling VerificationsApi->exchange_verification: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **verification_exchange_request** | [**VerificationExchangeRequest**](VerificationExchangeRequest.md)|  | 

### Return type

[**VerificationExchangeResponse**](VerificationExchangeResponse.md)

### Authorization

[bearerAuth](../README.md#bearerAuth)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | The verification this token proves. |  -  |
**401** | Missing or invalid API key (also returned for a publishable key used here, a disabled app, or a suspended company). |  -  |
**404** | Verification token not found, expired, or already exchanged. The same 404 covers a token that is not yours and one already exchanged by a different key of your own, so the endpoint cannot be used to probe which tokens exist. |  -  |
**422** | Request body failed validation. |  -  |
**503** | The platform is closed for maintenance. Retry after the interval in the Retry-After header. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

