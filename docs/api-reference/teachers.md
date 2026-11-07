# Teachers

Use the teacher endpoints to create, retrieve, update and delete teacher records.

All endpoints require a valid access token. The required scope depends on the operation. See [Authentication](../authentication.md).

## Create a teacher

`POST /teachers`

**Required scope:** `rostering.write`

Creates a teacher record.

### Request body

```json
{
  "external_id": "staff-0081",
  "school_id": "school_456",
  "first_name": "Alex",
  "last_name": "Morgan",
  "email": "alex.morgan@example.edu"
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `external_id` | string | Yes | Identifier from the integrating system. Unique within the school. |
| `school_id` | string | Yes | ID of the school the teacher belongs to. |
| `first_name` | string | Yes | Teacher's first name. |
| `last_name` | string | Yes | Teacher's last name. |
| `email` | string | No | Teacher's email address. |

### Response

`201 Created`

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

## Retrieve a teacher

`GET /teachers/{teacher_id}`

**Required scope:** `rostering.read`

Returns the teacher identified by `teacher_id`.

### Response

`200 OK` with the teacher object.

Returns `404 Not Found` if the teacher does not exist or is not accessible to the client.

## Update a teacher

`PUT /teachers/{teacher_id}`

**Required scope:** `rostering.write`

Replaces the teacher's editable fields with the values in the request body. Supply all required fields, including `external_id`, `school_id`, `first_name` and `last_name`.

### Request body

Use the same fields as [Create a teacher](#create-a-teacher).

### Response

`200 OK` with the updated teacher object.

## Delete a teacher

`DELETE /teachers/{teacher_id}`

**Required scope:** `rostering.write`

Deletes the specified teacher.

### Response

`204 No Content`

A successful deletion has no response body. Returns `404 Not Found` if the teacher does not exist or is not accessible to the client.
