# ErrorError


## Properties

Name | Type | Description | Notes
------------ | ------------- | ------------- | -------------
**type** | **str** | Machine-readable error class, e.g. \&quot;ApiKeyError\&quot;. | 
**message** | **str** |  | 
**details** | **Dict[str, object]** |  | [optional] 

## Example

```python
from otp_sdk.models.error_error import ErrorError

# TODO update the JSON string below
json = "{}"
# create an instance of ErrorError from a JSON string
error_error_instance = ErrorError.from_json(json)
# print the JSON string representation of the object
print(ErrorError.to_json())

# convert the object into a dict
error_error_dict = error_error_instance.to_dict()
# create an instance of ErrorError from a dict
error_error_from_dict = ErrorError.from_dict(error_error_dict)
```
[[Back to Model list]](../README.md#documentation-for-models) [[Back to API list]](../README.md#documentation-for-api-endpoints) [[Back to README]](../README.md)


