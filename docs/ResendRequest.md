# ResendRequest


## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**otp_id** | **UUID** |  | 
**channel** | **str** | Move this OTP onto a specific channel, e.g. \&quot;sms\&quot; when the recipient has no WhatsApp. The channel must be enabled for your app and the recipient. Omit to advance to the next channel in your routing order.  | [optional] 

## Example

```python
from otp_sdk.models.resend_request import ResendRequest

# TODO update the JSON string below
json = "{}"
# create an instance of ResendRequest from a JSON string
resend_request_instance = ResendRequest.from_json(json)
# print the JSON string representation of the object
print(ResendRequest.to_json())

# convert the object into a dict
resend_request_dict = resend_request_instance.to_dict()
# create an instance of ResendRequest from a dict
resend_request_from_dict = ResendRequest.from_dict(resend_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


