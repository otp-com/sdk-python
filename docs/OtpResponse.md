# OtpResponse


## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**otp_id** | **UUID** |  | 
**status** | [**Status**](Status.md) |  | 
**channel** | [**Channel**](Channel.md) | Channel the OTP was dispatched on; null until routed. | 
**masked_recipient** | **str** | Recipient with the middle digits masked. | 
**action_url** | **str** | WhatsApp link the user opens to receive the code: they send us the prefilled message and we reply with the code over WhatsApp, then they enter it and you call POST /otp/verify. Present only when dispatched on the whatsapp channel; null otherwise. | [optional] 

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


