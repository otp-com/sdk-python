# otp_sdk.OTPApi

All URIs are relative to *https://api.otp.com*

Method | HTTP request | Description
------------- | ------------- | -------------
[**get_otp_status**](OTPApi.md#get_otp_status) | **GET** /api/v1/otp/{otp_id} | Fetch the current status of an OTP.
[**resend_otp**](OTPApi.md#resend_otp) | **POST** /api/v1/otp/resend | Resend a pending OTP, escalating the channel if configured.
[**send_otp**](OTPApi.md#send_otp) | **POST** /api/v1/otp/send | Start an OTP: routes a channel and dispatches the code.
[**verify_otp**](OTPApi.md#verify_otp) | **POST** /api/v1/otp/verify | Verify a code against a pending OTP.


# **get_otp_status**
> OtpStatusResponse get_otp_status(otp_id)

Fetch the current status of an OTP.

### Example

* Bearer Authentication (bearerAuth):

```python
import otp_sdk
from otp_sdk.models.otp_status_response import OtpStatusResponse
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
    api_instance = otp_sdk.OTPApi(api_client)
    otp_id = UUID('38400000-8cf0-11bd-b23e-10b96e4ef00d') # UUID | 

    try:
        # Fetch the current status of an OTP.
        api_response = api_instance.get_otp_status(otp_id)
        print("The response of OTPApi->get_otp_status:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling OTPApi->get_otp_status: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **otp_id** | **UUID**|  | 

### Return type

[**OtpStatusResponse**](OtpStatusResponse.md)

### Authorization

[bearerAuth](../README.md#bearerAuth)

### HTTP request headers

 - **Content-Type**: Not defined
 - **Accept**: application/json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Current status of the OTP. |  -  |
**400** | otp_id is not a valid UUID. |  -  |
**401** | Missing or invalid API key (also returned for a disabled app or a suspended company). |  -  |
**404** | OTP not found (the same 404 is returned for an OTP belonging to another company). |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **resend_otp**
> OtpResponse resend_otp(resend_request)

Resend a pending OTP, escalating the channel if configured.

### Example

* Bearer Authentication (bearerAuth):

```python
import otp_sdk
from otp_sdk.models.otp_response import OtpResponse
from otp_sdk.models.resend_request import ResendRequest
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
    api_instance = otp_sdk.OTPApi(api_client)
    resend_request = otp_sdk.ResendRequest() # ResendRequest | 

    try:
        # Resend a pending OTP, escalating the channel if configured.
        api_response = api_instance.resend_otp(resend_request)
        print("The response of OTPApi->resend_otp:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling OTPApi->resend_otp: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **resend_request** | [**ResendRequest**](ResendRequest.md)|  | 

### Return type

[**OtpResponse**](OtpResponse.md)

### Authorization

[bearerAuth](../README.md#bearerAuth)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Resend accepted; the OTP may now be on a different channel. |  -  |
**401** | Missing or invalid API key (also returned for a disabled app or a suspended company). |  -  |
**404** | OTP not found (the same 404 is returned for an OTP belonging to another company). |  -  |
**409** | The OTP cannot be resent (resolved, expired, or out of attempts), or the requested channel is not enabled. |  -  |
**422** | Request body failed validation. |  -  |
**429** | Resend cooldown has not elapsed; see the Retry-After header. |  -  |
**503** | Routing picked WhatsApp but our inbound number is not configured. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **send_otp**
> OtpResponse send_otp(send_request, idempotency_key=idempotency_key)

Start an OTP: routes a channel and dispatches the code.

Routing picks the channel from the app config. When it selects WhatsApp the code is not sent yet: the response returns action_url (a wa.me link) the user opens to receive the code over WhatsApp, and the OTP stays pending until they enter it. On all other channels the code is delivered directly and action_url is null. Either way the user enters the code and you call POST /otp/verify.

### Example

* Bearer Authentication (bearerAuth):

```python
import otp_sdk
from otp_sdk.models.otp_response import OtpResponse
from otp_sdk.models.send_request import SendRequest
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
    api_instance = otp_sdk.OTPApi(api_client)
    send_request = otp_sdk.SendRequest() # SendRequest | 
    idempotency_key = 'idempotency_key_example' # str | Replay the prior response for a repeated request; a reused key with a different body is a 409. (optional)

    try:
        # Start an OTP: routes a channel and dispatches the code.
        api_response = api_instance.send_otp(send_request, idempotency_key=idempotency_key)
        print("The response of OTPApi->send_otp:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling OTPApi->send_otp: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **send_request** | [**SendRequest**](SendRequest.md)|  | 
 **idempotency_key** | **str**| Replay the prior response for a repeated request; a reused key with a different body is a 409. | [optional] 

### Return type

[**OtpResponse**](OtpResponse.md)

### Authorization

[bearerAuth](../README.md#bearerAuth)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**201** | OTP created and delivery started. |  -  |
**401** | Missing or invalid API key (also returned for a disabled app or a suspended company). |  -  |
**409** | No channel can reach this recipient, or the idempotency key was reused with a different body. |  -  |
**422** | Request body failed validation. |  -  |
**503** | Routing picked WhatsApp but our inbound number is not configured. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **verify_otp**
> VerifyResponse verify_otp(verify_request)

Verify a code against a pending OTP.

### Example

* Bearer Authentication (bearerAuth):

```python
import otp_sdk
from otp_sdk.models.verify_request import VerifyRequest
from otp_sdk.models.verify_response import VerifyResponse
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
    api_instance = otp_sdk.OTPApi(api_client)
    verify_request = otp_sdk.VerifyRequest() # VerifyRequest | 

    try:
        # Verify a code against a pending OTP.
        api_response = api_instance.verify_otp(verify_request)
        print("The response of OTPApi->verify_otp:\n")
        pprint(api_response)
    except Exception as e:
        print("Exception when calling OTPApi->verify_otp: %s\n" % e)
```



### Parameters


Name | Type | Description  | Notes
------------- | ------------- | ------------- | -------------
 **verify_request** | [**VerifyRequest**](VerifyRequest.md)|  | 

### Return type

[**VerifyResponse**](VerifyResponse.md)

### Authorization

[bearerAuth](../README.md#bearerAuth)

### HTTP request headers

 - **Content-Type**: application/json
 - **Accept**: application/json

### HTTP response details

| Status code | Description | Response headers |
|-------------|-------------|------------------|
**200** | Verification result. |  -  |
**401** | Missing or invalid API key (also returned for a disabled app or a suspended company). |  -  |
**404** | OTP not found (the same 404 is returned for an OTP belonging to another company). |  -  |
**422** | Request body failed validation. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

