# SendRequest


## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**recipient** | **str** | Phone number (E.164) or email address to deliver the OTP to. | 
**locale** | **str** | BCP-47 locale for the message template; falls back to the app default. | [optional] 

## Example

```python
from otp_sdk.models.send_request import SendRequest

# TODO update the JSON string below
json = "{}"
# create an instance of SendRequest from a JSON string
send_request_instance = SendRequest.from_json(json)
# print the JSON string representation of the object
print(SendRequest.to_json())

# convert the object into a dict
send_request_dict = send_request_instance.to_dict()
# create an instance of SendRequest from a dict
send_request_from_dict = SendRequest.from_dict(send_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


