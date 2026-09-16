# VerificationExchangeResponse


## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**otp_id** | **UUID** | The OTP this verification belongs to. | 
**recipient** | **str** | The recipient that was verified, in full. This is the answer the device could not be trusted to give you. | 
**recipient_type** | [**RecipientType**](RecipientType.md) |  | 
**channel** | [**Channel**](Channel.md) | Channel the verified code was delivered on. | 
**verified_at** | **datetime** | When the end user entered the correct code. | 

## Example

```python
from otp_sdk.models.verification_exchange_response import VerificationExchangeResponse

# TODO update the JSON string below
json = "{}"
# create an instance of VerificationExchangeResponse from a JSON string
verification_exchange_response_instance = VerificationExchangeResponse.from_json(json)
# print the JSON string representation of the object
print(VerificationExchangeResponse.to_json())

# convert the object into a dict
verification_exchange_response_dict = verification_exchange_response_instance.to_dict()
# create an instance of VerificationExchangeResponse from a dict
verification_exchange_response_from_dict = VerificationExchangeResponse.from_dict(verification_exchange_response_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


