# OtpResponse


## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**otp_id** | **UUID** |  | 
**status** | [**Status**](Status.md) |  | 
**channel** | [**Channel**](Channel.md) |  | 
**masked_recipient** | **str** | Recipient with the middle masked, e.g. +14****71. | 
**action_url** | **str** | WhatsApp link the user opens to receive the code: they send us the prefilled message and we reply with the code over WhatsApp. Present only when the OTP was dispatched on the whatsapp channel; null otherwise. Verification is the same on every channel: the user enters the code and you call &#x60;/otp/verify&#x60;.  | [optional] 

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


