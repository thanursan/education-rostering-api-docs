# Students

Use the student endpoints to create, retrieve, update and delete student records.

All endpoints require a valid access token. The required scope depends on the operation. See [Authentication](../authentication.md).

## Create a student

`POST /students`

**Required scope:** `rostering.write`

Creates a student record.

### Request body

```json
{
  "external_id": "pupil-1042",
  "school_id": "school_456",
  "first_name": "Jamie",
  "last_name": "Lee",
  "grade": 4
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `external_id` | string | Yes | Identifier from the integrating system. Unique within the school. |
| `school_id` | string | Yes | ID of the school the student belongs to. |
| `first_name` | string | Yes | Student's first name. |
| `last_name` | string | Yes | Student's last name. |
| `grade` | integer | Yes | Student's grade level. Must be between `0` and `6`. |

### Response

`201 Created`

```json
{
  "id": "student_789",
  "external_id": "pupil-1042",
  "school_id": "school_456",
  "first_name": "Jamie",
  "last_name": "Lee",
  "grade": 4
}
```

## Retrieve a student

`GET /students/{student_id}`

**Required scope:** `rostering.read`

Returns the student identified by `student_id`.

### Response

`200 OK` with the student object.

Returns `404 Not Found` if the student does not exist or is not accessible to the client.

## Update a student

`PUT /students/{student_id}`

**Required scope:** `rostering.write`

Replaces the student's editable fields with the values in the request body. Supply all required fields, including `external_id`, `school_id`, `first_name`, `last_name` and `grade`.

### Request body

Use the same fields as [Create a student](#create-a-student).

### Response

`200 OK` with the updated student object.

## Delete a student

`DELETE /students/{student_id}`

**Required scope:** `rostering.write`

Deletes the specified student.

### Response

`204 No Content`

A successful deletion has no response body. Returns `404 Not Found` if the student does not exist or is not accessible to the client.
