# Error handling

The API uses standard HTTP status codes and returns a JSON error object when a request fails.

## Error response

Error responses use a consistent structure:

```json
{
  "error": {
    "code": "validation_error",
    "message": "The request contains an invalid field.",
    "details": [
      {
        "field": "grade",
        "issue": "must be between 0 and 6"
      }
    ]
  }
}
```

| Property | Type | Description |
|---|---|---|
| `error.code` | string | Machine-readable error identifier. |
| `error.message` | string | Human-readable summary of the error. |
| `error.details` | array | Optional field-level details. |

## HTTP status codes

| Status | Meaning | Typical cause |
|---|---|---|
| `200 OK` | Request succeeded. | Resource retrieved or updated. |
| `201 Created` | Resource created. | A teacher, student or class was created. |
| `202 Accepted` | Request accepted for processing. | A rostering job was queued. |
| `204 No Content` | Request succeeded without a response body. | Resource deleted. |
| `400 Bad Request` | Request is invalid. | Malformed JSON or invalid field value. |
| `401 Unauthorized` | Authentication failed. | Missing, invalid or expired access token. |
| `403 Forbidden` | Access is not permitted. | Required scope is missing. |
| `404 Not Found` | Resource was not found. | Unknown ID or inaccessible resource. |
| `409 Conflict` | Request conflicts with existing data. | Duplicate `external_id` within a school. |
| `415 Unsupported Media Type` | Content type is unsupported. | Request body is not JSON. |
| `500 Internal Server Error` | Unexpected server error. | Unhandled service failure. |

## Handle errors

When an API request fails:

1. Check the HTTP status code.
2. Read `error.code` to identify the error category.
3. If `error.details` is present, use it to identify invalid fields.
4. Correct the request before retrying a client-side error.
5. For unexpected server errors, retry according to your integration's retry policy.

Do not repeatedly retry requests that fail with `400`, `401`, `403`, `404` or `409` without changing the request or resolving the underlying issue.

## Example: validation error

A request with an unsupported grade value might return:

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json
```

```json
{
  "error": {
    "code": "validation_error",
    "message": "The request contains an invalid field.",
    "details": [
      {
        "field": "grade",
        "issue": "must be between 0 and 6"
      }
    ]
  }
}
```
