# otp_sdk.OTPApi

All URIs are relative to *https://api.otp.com/api/v1*

Method | HTTP request | Description
------------- | ------------- | -------------
[**get_otp_status**](OTPApi.md#get_otp_status) | **GET** /otp/{otp_id} | Get OTP status
[**resend_otp**](OTPApi.md#resend_otp) | **POST** /otp/resend | Resend an OTP
[**send_otp**](OTPApi.md#send_otp) | **POST** /otp/send | Send an OTP
[**verify_otp**](OTPApi.md#verify_otp) | **POST** /otp/verify | Verify an OTP


# **get_otp_status**
> OtpStatusResponse get_otp_status(otp_id)

Get OTP status

### Example

* Bearer Authentication (bearerAuth):

```python
import otp_sdk
from otp_sdk.models.otp_status_response import OtpStatusResponse
from otp_sdk.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to https://api.otp.com/api/v1
# See configuration.py for a list of all supported configuration parameters.
configuration = otp_sdk.Configuration(
    host = "https://api.otp.com/api/v1"
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
        # Get OTP status
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
**200** | Current status. |  -  |
**401** | Missing or invalid API key (also returned for a disabled app or suspended company). |  -  |
**404** | OTP not found (also returned for another company&#39;s OTP, to avoid probing). |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **resend_otp**
> OtpResponse resend_otp(resend_request)

Resend an OTP

Resend a pending OTP, advancing to the next configured channel (e.g. SMS to WhatsApp).

### Example

* Bearer Authentication (bearerAuth):

```python
import otp_sdk
from otp_sdk.models.otp_response import OtpResponse
from otp_sdk.models.resend_request import ResendRequest
from otp_sdk.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to https://api.otp.com/api/v1
# See configuration.py for a list of all supported configuration parameters.
configuration = otp_sdk.Configuration(
    host = "https://api.otp.com/api/v1"
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
        # Resend an OTP
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
**200** | Resend accepted. |  -  |
**401** | Missing or invalid API key (also returned for a disabled app or suspended company). |  -  |
**404** | OTP not found (also returned for another company&#39;s OTP, to avoid probing). |  -  |
**409** | No enabled channel, channel not enabled, resend not allowed, or an idempotency-key conflict. |  -  |
**429** | Resend cooldown not elapsed. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **send_otp**
> OtpResponse send_otp(send_request, idempotency_key=idempotency_key)

Send an OTP

Generate a one-time password and deliver it to the recipient. The channel is chosen by your app's routing (default order + per-country overrides). Returns an `otp_id` to verify against. When routing picks WhatsApp the code is not sent yet: the response carries an `action_url` (a wa.me link) the user opens to receive the code over WhatsApp, and the OTP stays pending until they enter it. On every channel the user enters the code and you call `/otp/verify`.


### Example

* Bearer Authentication (bearerAuth):

```python
import otp_sdk
from otp_sdk.models.otp_response import OtpResponse
from otp_sdk.models.send_request import SendRequest
from otp_sdk.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to https://api.otp.com/api/v1
# See configuration.py for a list of all supported configuration parameters.
configuration = otp_sdk.Configuration(
    host = "https://api.otp.com/api/v1"
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
    idempotency_key = 'idempotency_key_example' # str | Replays the prior response for the same key; a reused key with a different body is a 409. (optional)

    try:
        # Send an OTP
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
 **idempotency_key** | **str**| Replays the prior response for the same key; a reused key with a different body is a 409. | [optional] 

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
**401** | Missing or invalid API key (also returned for a disabled app or suspended company). |  -  |
**409** | No enabled channel, channel not enabled, resend not allowed, or an idempotency-key conflict. |  -  |
**422** | Request body failed validation. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

# **verify_otp**
> VerifyResponse verify_otp(verify_request)

Verify an OTP

Verify the code the user entered. `matched: true` means the code was correct.

### Example

* Bearer Authentication (bearerAuth):

```python
import otp_sdk
from otp_sdk.models.verify_request import VerifyRequest
from otp_sdk.models.verify_response import VerifyResponse
from otp_sdk.rest import ApiException
from pprint import pprint

# Defining the host is optional and defaults to https://api.otp.com/api/v1
# See configuration.py for a list of all supported configuration parameters.
configuration = otp_sdk.Configuration(
    host = "https://api.otp.com/api/v1"
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
        # Verify an OTP
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
**401** | Missing or invalid API key (also returned for a disabled app or suspended company). |  -  |
**404** | OTP not found (also returned for another company&#39;s OTP, to avoid probing). |  -  |
**422** | Request body failed validation. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to Model list]](../README.md#documentation-for-models) [[Back to README]](../README.md)

