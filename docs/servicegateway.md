# Introduction

This section included the extra information around usage of the Service Gateway Rest API. For general documentation please visit https://service-gateway/api/index.html to enter Swagger on you server.

# Authentication

When authenticating against the service gateway you will need to fetch an `OTDSTicket` (Open Text Directory Service Ticket) to be used in your following queries against the service gateway.

<h4>Endpoint</h4>
```html
https:// {{OTDS_SERVER}} /otdsws/v1/authentication/credentials
```

<h4>Header</h4>
```
Content-type: application/json
```

<h4>Body</h4>
The authentication must be passed in the body. **Be aware** of the `user_name` typed with an underscore.

```json
{
  "user_name"	:	"",
  "password"	:	""
}
```

<h4>Response</h4>

In the respons you will get the `ticket` among other values. Make sure to save this as you will need it when authenticating against the other services later.

```json
{
  "token": "",
  "user_id": "",
  "ticket": "",
  "resource_id": ,
  "failure_reason": ,
  "password_expiration_time": ,
  "continuation": ,
  "continuation_context": ,
  "continuation_data":
}
```

---

# Documents
We'll document the `v1` of the document searching and fetching.

<h2>Search for a document</h2>
More search variables can be found at Swagger Documentation

<h4>Endpoint</h4>
```html
https:// {{SG_SERVER}} /v1/documents
```

<h4>Header</h4>
```http-header
Content-type: application/json
Authorization: {{ OTDSTicket }}
```

<h4>URL Parameters</h4>

| URL Parameter | Required | Type    | Note                                                                              |
|---------------|----------|---------|-----------------------------------------------------------------------------------|
| where_typeid  | YES      | string  | Filter on specified document type eg. jobpart, inputqueueobject, archiveobject.   |
| where_filter  | NO       | string  | Filter on specified fields [RANGE|IN|EQ|GT|NEQ]<property|GUID>,<value>,[,].       |
| guid_format   | NO       | boolean | Is defaulted to true, for easier reading use `false` which return variable names. |

For all URL parameters, please check Swagger.


<h4>Response</h4>

In the respons you will get the `id` among other values. This id will need to be stored as we will use it to fetch the actual document. Do not decode the base64 as the `/documents` wants it in that format.

```json
{
  "status": "success",
  "data": [
    {
      "id": "**Base64 encoded value**",
      ...
    }
  ]
}
```

---

<h2>Download a specific document</h2>
This section is not specified in the Swagger API documentation.

<h4>Endpoint</h4>
```html
https:// {{SG_SERVER}} /v1/documents/{{ DOCUMENT_BASE64_ENCODED_ID }}/content
```

<h4>Header</h4>
```http-header
Content-type: application/json
Authorization: {{ OTDSTicket }}
```


<h4>Response</h4>

In the respons you will get the actual document together with the correct `Content-Type` that was also specified in the previous response.