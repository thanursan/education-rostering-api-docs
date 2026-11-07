# Classes

Use the class endpoints to create, retrieve, update and delete class records.

A class belongs to one school and can reference teachers and students. The referenced IDs must belong to the same school as the class.

All endpoints require a valid access token. The required scope depends on the operation. See [Authentication](../authentication.md).

## Create a class

`POST /classes`

**Required scope:** `rostering.write`

Creates a class record.

### Request body

```json
{
  "external_id": "class-4a",
  "school_id": "school_456",
  "name": "Class 4A",
  "grade": 4,
  "teacher_ids": ["teacher_123"],
  "student_ids": ["student_789", "student_790"]
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `external_id` | string | Yes | Identifier from the integrating system. Unique within the school. |
| `school_id` | string | Yes | ID of the school the class belongs to. |
| `name` | string | Yes | Class name. |
| `grade` | integer | Yes | Grade level, from `0` to `6`. |
| `teacher_ids` | array of strings | No | IDs of teachers assigned to the class. Defaults to an empty array. |
| `student_ids` | array of strings | No | IDs of students assigned to the class. Defaults to an empty array. |

### Response

`201 Created`

```json
{
  "id": "class_321",
  "external_id": "class-4a",
  "school_id": "school_456",
  "name": "Class 4A",
  "grade": 4,
  "teacher_ids": ["teacher_123"],
  "student_ids": ["student_789", "student_790"]
}
```

## Retrieve a class

`GET /classes/{class_id}`

**Required scope:** `rostering.read`

Returns the class identified by `class_id`, including its teacher and student IDs.

### Response

`200 OK` with the class object.

Returns `404 Not Found` if the class does not exist or is not accessible to the client.

## Update a class

`PUT /classes/{class_id}`

**Required scope:** `rostering.write`

Replaces the class's editable fields. Supply all required fields shown in the create request. If `teacher_ids` or `student_ids` are omitted, the corresponding assignments are cleared.

### Request body

Use the same fields as [Create a class](#create-a-class).

### Response

`200 OK` with the updated class object.

## Delete a class

`DELETE /classes/{class_id}`

**Required scope:** `rostering.write`

Deletes the specified class. This operation does not delete the associated teacher or student records.

### Response

`204 No Content`

A successful deletion has no response body. Returns `404 Not Found` if the class does not exist or is not accessible to the client.
