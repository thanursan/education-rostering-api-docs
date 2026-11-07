# Get started

This guide explains the conventions used in the Education Rostering API and shows how to make your first request.

## Base URL

All API resource endpoints use this base URL:

```text
https://api.example.com/v1
```

The API is versioned in the URL. The examples use `v1`.

## Requirements

Before sending requests, make sure you have:

- A client ID and client secret issued by the service administrator.
- Permission to access the required school data.
- An access token obtained using the [Client Credentials flow](authentication.md).

## Make a request

The following example retrieves a teacher by ID. This request requires the `rostering.read` scope. Replace the placeholder token and ID with valid values from your environment.

```http
GET /v1/teachers/teacher_123 HTTP/1.1
Host: api.example.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Accept: application/json
```

A successful request returns `200 OK` and a JSON representation of the teacher:

```json
{
  "id": "teacher_123",
  "external_id": "staff-0081",
  "school_id": "school_456",
  "first_name": "Alex",
  "last_name": "Morgan",
  "email": "alex.morgan@example.edu"
}
```

The identifiers and example data on this site are fictional.

## Request conventions

- Use HTTPS for all requests.
- Send JSON request bodies with `Content-Type: application/json`.
- Send `Accept: application/json` when requesting JSON responses.
- Include an access token in the `Authorization` header.
- Resource IDs are assigned by the API when a resource is created.
- `external_id` is the identifier used by the integrating system. It must be unique within its school.

## HTTP methods

| Method | Purpose |
|---|---|
| `GET` | Retrieve a resource |
| `POST` | Create a resource or submit a job |
| `PUT` | Replace an existing resource |
| `DELETE` | Delete a resource |

The resource reference pages describe the fields and responses for each endpoint.
