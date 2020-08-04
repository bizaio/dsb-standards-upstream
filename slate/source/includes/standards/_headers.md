## HTTP Headers

Supported HTTP headers, and their usage, for the standards are as laid out in the following sections.

### Request Headers
>A sample set of headers requesting version 3 to 5:  
`Content-Type : application/json`  
`Accept : application/json`  
`x-v : 5`  
`x-min-v : 3`  
`x-fapi-interaction-id : 6ba7b814-9dad-11d1-80b4-00c04fd430c8`  
`x-fapi-auth_date : 2020-01-16 16:50:15.507399`  
`x-fapi-customer-ip-address : 2001:0db8:85a3:0000:0000:8a2e:0370:7334`  
`x-cds-client-headers : TW96aWxsYS81LjAgKFgxMTsgTGludXggeDg2XzY0KSBBcHBsZVdlYktpdC81MzcuMzYgKEtIVE1MLCBsaWtlIEdlY2tvKSBDaHJvbWUvNzkuMC4zOTQ1Ljg4IFNhZmFyaS81MzcuMzY=`  


Header Field | Description | Mandatory?
-------------|-------------|-----------
**Content-Type** | Standard HTTP Header. Represents the format of the payload provided in the request. Must be set to `application/json`. Mandatory for PUT and POST calls.| Conditional
**Accept** | Standard HTTP Header. Specify the Content-Type that is required from the Server.<br/>If specified, must be set to `application/json` unless otherwise specified in the resource end point standard.<br/>If set to an unacceptable value the holder must respond with a 406 Not Acceptable. If not specified, default is `application/json`. | Optional
**x-v** | Version of the API end point requested by the client. Must be set to a positive integer. The holder should respond with the highest supported version between `x-min-v` and `x-v`. If the value of `x-min-v` is equal to or higher than the value of `x-v` then the `x-min-v` header should be treated as absent. <br/>If all versions requested are not supported then the holder must respond with a `406 Not Acceptable`. | Mandatory
**x-min-v** | Minimum version of the API end point requested by the client. Must be set to a positive integer if provided. The holder should respond with the highest supported version between `x-min-v` and `x-v`. If the value of `x-min-v` is equal to or higher than the value of `x-v` then the `x-min-v` header should be treated as absent. <br/>If all versions requested are not supported then the holder must respond with a `406 Not Acceptable`. | Optional
**x-&lt;HID&gt;-v** | A holder specific version of extension fields. Should not be used in conjunction with `x-min-v`. | Optional
**x-fapi-interaction-id** | An optional [[RFC4122](https://tools.ietf.org/html/rfc4122)] UUID used as a correlation id. If provided, the data holder must "play back" this value in the `x-fapi-interaction-id` response header.| Optional
**x-fapi-auth-date** | The time when the customer last logged in to the data recipient as described in [FAPI-R].  Required for all resource calls (customer present and unattended).   Not required for unauthenticated calls. | Conditional
**x-fapi-customer-ip-address** | The customer's original IP address if the customer is currently logged in to the data recipient. The presence of this header indicates that the API is being called in a customer present context. Not to be included for unauthenticated calls. | Conditional
**x-cds-client-headers** | The customer's original standard http headers [Base64](#common-field-types) encoded, including the original User Agent header, if the customer is currently logged in to the data recipient. Mandatory for customer present calls.  Not required for unattended or unauthenticated calls.<br/>This header is not required to include:<br/><ul><li>Headers containing security information</li><li>Custom or proprietary headers used to facilitate the client application</li></ul>| Conditional

### Response headers
Header Field | Description | Mandatory?
-------------|-------------|-----------
**Content-Type** | Standard HTTP Header. Represents the format of the payload returned in the response.<br/>Must be `application/json` unless otherwise specified in the resource end point standard. | Mandatory
**Retry-After** | Header indicating the time (in seconds) that the client should wait before retrying an operation. The holder should include this header along with responses with the HTTP status code of `429 Too many requests`. | Optional
**x-v** | The version of the API end point that the holder has responded with. | Mandatory
**x-fapi-interaction-id** | An [[RFC4122](https://tools.ietf.org/html/rfc4122)] UUID used as a correlation id. The data holder must set the response header `x-fapi-interaction-id` to the value received from the corresponding request header or to a new [RFC4122] UUID value if the request header was not provided.| Mandatory

### Additional Headers

Generally understood headers used in HTTP transactions to provide caching guidance and the use of the compression are not specified but are considered acceptable. It is at the discretion of the data holder if these headers are used for a specific implementation. Data holders should not require these headers for successful API access, however.
