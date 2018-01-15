This document describes how impersonation works using the REST Api.


# Impersonation

Impersonation allows a system or user that interacts with the REST Api to act as another user.

## Prerequisites

For impersonation the `IMPERSONATE` right is required.

## Example

First, the user that will request the impersonation token needs to be authenticated using one of the authentication mechanisms described [here](Authentication).

*Code sample*

```powershell
$basicCredentials = "USERNAME:PASSWORD";
$base64EncodedCredentials = $basicCredentials | ConvertTo-Base64;
$basicAuthHeaderValue = "Basic {0}" -f $base64EncodedCredentials;

$headers = @{
	Authorization = $basicAuthHeaderValue;
	"Content-Type" = "application/json";
};

Invoke-RestMethod -Method Post -Uri "http://appclusive/api/Core/Authentications/BasicLogin" -Headers $headers;
```

After successful authentication a JSON Web Token (JWT) for the user to be impersonated can be requested as follows.

*Code sample*

```powershell
$userId = "ID_OF_THE_USER_TO_BE_IMPERSONATED";

$body = @{ UserId = $userId } | ConvertTo-Json;

$result = Invoke-RestMethod -Method Post -Uri "http://appclusive/api/Core/Authentications/Impersonate" -Headers $headers -Body $body;
$jwt = $result.Token;
```

The JSON Web Token returned by the `/Impersonate` action can now be used to interact with the API as the specified user.

*Code sample*

```powershell
$bearerAuthHeader = "Bearer {0}" -f $jwt;
$headers = @{
	Authorization = $bearerAuthHeader;
};

Invoke-RestMethod -Method Get -Uri "http://appclusive/api/Core/Users" -Headers $headers;
```
