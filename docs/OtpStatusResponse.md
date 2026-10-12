# OtpStatusResponse


## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**otp_id** | **UUID** |  | 
**status** | [**Status**](Status.md) |  | 
**masked_recipient** | **str** | Recipient with the middle digits masked. | 
**completion** | [**Completion**](Completion.md) | How this OTP completes. \&quot;code\&quot;: the user enters the code and you call POST /otp/verify. \&quot;inbound\&quot;: the user opens action_url and sends the prefilled WhatsApp message; that message approves the OTP, no code is sent. Poll GET /otp/:id or take the otp.approved webhook. | 

## Example

```python
from otp_sdk.models.otp_status_response import OtpStatusResponse

# TODO update the JSON string below
json = "{}"
# create an instance of OtpStatusResponse from a JSON string
otp_status_response_instance = OtpStatusResponse.from_json(json)
# print the JSON string representation of the object
print(OtpStatusResponse.to_json())

# convert the object into a dict
otp_status_response_dict = otp_status_response_instance.to_dict()
# create an instance of OtpStatusResponse from a dict
otp_status_response_from_dict = OtpStatusResponse.from_dict(otp_status_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


