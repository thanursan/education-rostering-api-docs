# Rostering jobs

Use the rostering jobs endpoint to submit a batch of teacher, student and class changes for asynchronous processing.

Unlike the individual resource endpoints, this operation returns a job ID. The job is processed after the request is accepted.

See [Authentication](../authentication.md) for information about obtaining an access token.

## Submit a rostering job

`POST /rostering/jobs`

**Required scope:** `rostering.write`

### Request body

Each collection is optional, but at least one collection must contain an item.

```json
{
  "school_id": "school_456",
  "callback_url": "https://integration.example.edu/webhooks/rostering",
  "teachers": [
    {
      "external_id": "staff-0081",
      "first_name": "Alex",
      "last_name": "Morgan",
      "email": "alex.morgan@example.edu"
    }
  ],
  "students": [
    {
      "external_id": "pupil-1042",
      "first_name": "Jamie",
      "last_name": "Lee",
      "grade": 4
    }
  ],
  "classes": [
    {
      "external_id": "class-4a",
      "name": "Class 4A",
      "grade": 4,
      "teacher_external_ids": ["staff-0081"],
      "student_external_ids": ["pupil-1042"]
    }
  ]
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `school_id` | string | Yes | School to which all submitted records belong. |
| `callback_url` | string | No | HTTPS URL that receives a notification when processing finishes. |
| `teachers` | array | No | Teacher records to create or update. |
| `students` | array | No | Student records to create or update. |
| `classes` | array | No | Class records and their teacher/student associations to create or update. |

For batch submissions, each record uses `external_id` to identify the corresponding record within the specified school. Existing records are updated; records that do not exist are created. Class associations use the external IDs supplied in the same school.

### Response

`202 Accepted`

```json
{
  "job_id": "job_abc123",
  "status": "queued",
  "status_url": "/v1/rostering/jobs/job_abc123"
}
```

A `202 Accepted` response means the job has been accepted for processing. It does not mean that every record has been successfully created or updated.

## Retrieve job status

`GET /rostering/jobs/{job_id}`

**Required scope:** `rostering.read`

Returns the current status and, when processing is complete, a summary of the results.

### Response while processing

`200 OK`

```json
{
  "job_id": "job_abc123",
  "status": "processing"
}
```

### Completed response

`200 OK`

```json
{
  "job_id": "job_abc123",
  "status": "completed",
  "summary": {
    "teachers": { "created": 1, "updated": 0, "failed": 0 },
    "students": { "created": 1, "updated": 0, "failed": 0 },
    "classes": { "created": 1, "updated": 0, "failed": 0 }
  },
  "errors": []
}
```

The `status` field can be `queued`, `processing`, `completed` or `failed`. A completed job can contain record-level failures.

The `errors` array contains details of records that could not be processed. Its entries identify the affected resource and use the same error structure as the API's standard error response.

For example:

```json
{
  "resource": "student",
  "external_id": "pupil-1042",
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

Clients can poll this endpoint to retrieve results whether or not a callback URL was supplied.

## Completion callback

If `callback_url` is included, the service sends a `POST` request to that URL when the job reaches a terminal status (`completed` or `failed`).

Example payload:

```json
{
  "job_id": "job_abc123",
  "status": "completed",
  "summary": {
    "teachers": { "created": 1, "updated": 0, "failed": 0 },
    "students": { "created": 1, "updated": 0, "failed": 0 },
    "classes": { "created": 1, "updated": 0, "failed": 0 }
  },
  "errors": []
}
```

The receiving integration should return a `2xx` response to acknowledge receipt.

If delivery fails, the service retries the callback up to three times. The receiving integration should handle duplicate notifications using `job_id` as the identifier.

If callback delivery remains unsuccessful, clients can retrieve the job status and results using the status endpoint.

!!! note "Callback security"
    This sample does not specify a webhook-signature scheme. A production API specification should document callback authentication and signature verification before implementation.
