# VerificationExchangeRequest


## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**verification_token** | **str** | The verification_token your app received from POST /client/otp/verify. | 

## Example

```python
from otp_sdk.models.verification_exchange_request import VerificationExchangeRequest

# TODO update the JSON string below
json = "{}"
# create an instance of VerificationExchangeRequest from a JSON string
verification_exchange_request_instance = VerificationExchangeRequest.from_json(json)
# print the JSON string representation of the object
print(VerificationExchangeRequest.to_json())

# convert the object into a dict
verification_exchange_request_dict = verification_exchange_request_instance.to_dict()
# create an instance of VerificationExchangeRequest from a dict
verification_exchange_request_from_dict = VerificationExchangeRequest.from_dict(verification_exchange_request_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


