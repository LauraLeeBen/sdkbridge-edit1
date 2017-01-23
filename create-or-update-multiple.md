label: Create or Update Multiple Markings
id: api_mgmt_markings_createmany
categorySlug: api-management-markings

Create or Update Multiple Markings
=======

Creates or updates one or more data markings.

Request
-------

### URL

```    
/v2/{tenant_id}/markings
```

where `{tenant_id}` is a 24-digit identifier assigned to the Ionic customer tenant.

### Method

```   
POST
```

### Headers

| Header | Required | Notes |
| ------ | -------- | ----- |
| `Authorization: Basic <base64 user+password>` | Required | Token used for authorization. See [Authentication and Authorization](#api_mgmt_markings_intro). |
| `Content-Type: application/json` | Required | Valid request bodies are in JSON format. |
| `Accept-Encoding: compress/gzip` | Optional | Returns compressed response |
| `Accepts: application/json` | Optional | Returns response in JSON format |

### Request Body

The request body is an array of JSON objects representing markings to be created or updated. A marking is only updated if it includes the `id` field and references an existing marking with that ID. If no `id` field is supplied, then a new marking is created.

#### Example

```json
[
    {
        "name": "new-marking",
        "public": true
        "adminOnly": true,
        "detail": {
            "dataType": "string",
            "description": "New corporate marking",
            "values": [
                {
                    "description": "An important item",
                    "name": "important"
                }
            ]
        }
    },
    {
        "id": "52222222222222222222222",
        "name": "update-this-marking-name",
        "public": false
    }
]
```

#### Fields

**Note:** For updates, fields that are excluded from the request body will have their original values preserved in the existing marking object.

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| id | string | Optional | `""` | Unique database identifier for the marking |
| name | string | Optional | `""` | Marking name. For a new marking, this is required. For an existing marking, it is optional. The value must be case-insenstively unique across set of existing markings. Used as an attribute-id in policy. |
| public | boolean | Optional | `false` | If `true`, this marking and its values can be provided to registered devices that request a list of markings. Useful for providing users with a list of markings to select to assign to protected content. |
| adminOnly | boolean | Optional | `false` | If `true`, values can only be administratively added to this marking. If `false`, values will be collected whenever a registered device protects content using this marking. Generally, a `public=true` marking also is configured `adminOnly=true`. | 
| detail/dataType | string | Optional | `"string"` | Only `string` is supported. |
| detail/description | string | Optional | `""` | Description of the marking. |
| detail/values | array | Optional | `null` | Array of marking values. If the array is provided, this array will overwrite existing values for this marking.  |
| detail/values/name | string | Required | | Unique name used as key and policy attribute value |
| detail/values/description | string | Optional | `""` | Description of the marking value |

### Sample Request

```
POST https://api.ionic.com/v2/555555555555555555555555/markings
Authorization: Basic QWxhZGRpbjpPcGVuU2VzYW1l
Content-Type:  application/json
[
    {
        "id": "533333333333333333333333",
        "name": "new-marking",
        "public": true,
        "adminOnly": true,
        "detail": {
            "dataType": "string",
            "default": false,
            "description": "New corporate marking",
            "values": [
                {
                    "adminCreated": true,
                    "description": "An important item",
                    "name": "important",
                    "policies": {
                        "referenced": 0,
                        "relevant": 3
                    }
                }
            ]
        },
        "_version": 1,
        "_createdTs": 1470329374,
        "_updatedTs": 1470329374
    },
    {
        "id": "52222222222222222222222",
        "name": "update-this-marking-name",
        "public": false,
        "adminOnly": true,
        "detail": {
            "dataType": "string",
            "default": false,
            "description": "Existing marking",
            "values": [
                {
                    "adminCreated": true,
                    "description": An existing marking value to be preserved",
                    "name": "existing",
                    "policies": {
                        "referenced": 1,
                        "relevant": 3
                    }
                }
            ]
        },
        "_version": 3,
        "_createdTs": 1470278938,
        "_updatedTs": 1470329374
    }
]

```

curl:

```
curl {tbd}
```

Response
--------

### Response Content Example

```json
[
    {
        "id": "533333333333333333333333",
        "name": "new-marking",
        "public": true,
        "adminOnly": true,
        "detail": {
            "dataType": "string",
            "default": false,
            "description": "New corporate marking",
            "values": [
                {
                    "adminCreated": true,
                    "description": "An important item",
                    "name": "important",
                    "policies": {
                        "referenced": 0,
                        "relevant": 3
                    }
                }
            ]
        },
        "_version": 1,
        "_createdTs": 1470329374,
        "_updatedTs": 1470329374
    },
    {
        "id": "52222222222222222222222",
        "name": "update-this-marking-name",
        "public": false,
        "adminOnly": true,
        "detail": {
            "dataType": "string",
            "default": false,
            "description": "Existing marking",
            "values": [
                {
                    "adminCreated": true,
                    "description": An existing marking value to be preserved",
                    "name": "existing",
                    "policies": {
                        "referenced": 1,
                        "relevant": 3
                    }
                }
            ]
        },
        "_version": 3,
        "_createdTs": 1470278938,
        "_updatedTs": 1470329374
    }
]
```

### Response Content Fields

| Field | Type | Description |
|-------|------|-------------|
| id | string | Unique database identifier for the marking |
| name | string | Marking name. Used as an attribute-id in policy |
| public | boolean | If `true`, this marking and its values can be provided to registered devices that request a list of markings. Useful for providing users with a list of markings to select to assign to protected content. |
| adminOnly | boolean | If `true`, values can only be administratively added to this marking. If `false`, values will be collected whenever a registered device protects content using this marking. Generally, a `public=true` marking also is configured `adminOnly=true`. | 
| detail/dataType | string | Only `string` is supported. |
| detail/default | boolean | If `true`, the marking is built-in to the Ionic platform and cannot be removed |
| detail/description | string | Description of the marking. |
| detail/values | array | Array of marking values |
| detail/values/name | string | Unique name used as key and policy attribute value |
| detail/values/adminCreated | boolean | If `true`,the value was originally created administratively. If `false`, the value was collected when a registered device protected content using this marking-value combination. |
| detail/values/description | string | Description of the marking value |
| detail/values/policies | object | Summary of this marking value's use in policy |
| detail/values/policies/referenced | integer | Number of policies that explicitly reference this marking value |
| detail/values/policies/relevant | integer | Number of policies that would be applied to a fetch request for a key tagged with this marking value |
| _version | integer | An integer version that is incremented whenever this object is changed |
| _createdTs | integer | The creation timestamp for this marking, expressed as seconds since Jan 01 1970. (UTC) |
| _updatedTs | integer | The timestamp for the most recent update to this marking or its values, expressed as seconds since Jan 01 1970. (UTC) |


### Status Codes and Errors

| Code | Causes |
| ---- | ------ |
| 200 OK | Successfully created and/or updated |
| 400 Bad Request | Invalid JSON |
| 400 Bad Request | Extra fields included in the request body that are not expected or defined in the fields table |
| 401 Unauthorized | Missing or invalid `Authorization` header |
| 403 Forbidden | The authenticated user is not assigned to a role that is granted API access or create access to markings |
| 404 Not Found | Marking with the `id` provided in the request body does not exist |
| 409 Conflict | The name provided is already in use |
| 409 Conflict | Cannot rename a marking that is explicitly referenced by a data policy |
| 409 Conflict | Cannot update a marking resulting in the removal of a marking value that is explicitly referenced by a data policy |
| 409 Conflict | Cannot change name of a default Ionic-provided marking |



