# Authentication

The Education Rostering API uses OAuth 2.0 Client Credentials to authenticate server-to-server requests.

You must obtain an access token and include it in every API request.

## Obtain an access token

Send a `POST` request to the token endpoint:

```text
https://auth.example.com/oauth2/token
```

Include the `scope` parameter to request the permissions required by your integration. The requested scopes must be permitted for your client.

Set the content type to `application/x-www-form-urlencoded` and provide your client credentials.

```http
POST /oauth2/token HTTP/1.1
Host: auth.example.com
Content-Type: application/x-www-form-urlencoded
```

```text
grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET&scope=rostering.read%20rostering.write
```

A successful response returns an access token:

```json
{
  "access_token": "eyJhbGciOi...example",
  "token_type": "Bearer",
  "expires_in": 3600,
  "scope": "rostering.read rostering.write"
}
```

The token value above is illustrative and cannot be used to authenticate.

## Use the access token

Include the token in the `Authorization` header:

```http
Authorization: Bearer YOUR_ACCESS_TOKEN
```

For example:

```bash
curl --request GET \
  --url https://api.example.com/v1/teachers/teacher_123 \
  --header "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  --header "Accept: application/json"
```

## Token expiry

Access tokens expire after the number of seconds specified by `expires_in`. The example response uses a lifetime of 3,600 seconds.

Request a new token when the current token expires. Do not wait for an authentication failure before refreshing it if your integration can determine its expiry time.

## Scopes

The API defines these scopes:

| Scope | Permission |
|---|---|
| `rostering.read` | Retrieve teachers, students, classes and job status |
| `rostering.write` | Create, update and delete resources and submit rostering jobs |

A client must be granted the relevant scope by the service administrator.

## Security recommendations

- Keep client credentials and access tokens confidential.
- Store secrets in a secure secret-management system.
- Do not include credentials in source code or logs.
- Use HTTPS and validate the server certificate.
- Request only the scopes required by the integration.
