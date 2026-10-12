# OtpResponse


## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**otp_id** | **UUID** |  | 
**status** | [**Status**](Status.md) |  | 
**channel** | [**Channel**](Channel.md) | Channel the OTP was dispatched on; null until routed. | 
**masked_recipient** | **str** | Recipient with the middle digits masked. | 
**action_url** | **str** | WhatsApp link the user opens: they send us the prefilled message and we reply with the code over WhatsApp (completion \&quot;code\&quot;) or approve the OTP on that message (completion \&quot;inbound\&quot;). Present only when dispatched on the whatsapp channel; null otherwise. Null under the template mode, where the code is pushed directly and the flow is the same as SMS. | [optional] 
**completion** | [**Completion**](Completion.md) | How this OTP completes. \&quot;code\&quot;: the user enters the code and you call POST /otp/verify. \&quot;inbound\&quot;: the user opens action_url and sends the prefilled WhatsApp message; that message approves the OTP, no code is sent. Poll GET /otp/:id or take the otp.approved webhook. | 

## Example

```python
from otp_sdk.models.otp_response import OtpResponse

# TODO update the JSON string below
json = "{}"
# create an instance of OtpResponse from a JSON string
otp_response_instance = OtpResponse.from_json(json)
# print the JSON string representation of the object
print(OtpResponse.to_json())

# convert the object into a dict
otp_response_dict = otp_response_instance.to_dict()
# create an instance of OtpResponse from a dict
otp_response_from_dict = OtpResponse.from_dict(otp_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


