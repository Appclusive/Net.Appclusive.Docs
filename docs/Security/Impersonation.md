This document describes how impersonation works using the REST Api.


# Impersonation

## Prerequisites

To impersonate another user the `IMPERSONATE` right is required.

## Example

The following code sample uses basic authentication however impersonation is also possible with any of the supported authentication mechanisms described [here](Authentication).

*Code sample*

```powershell
$userId = "ID_OF_THE_USER_TO_BE_IMPERSONATED";
$basicCredentials = "USERNAME:PASSWORD";
$base64EncodedCredentials = $basicCredentials | ConvertTo-Base64;
$basicAuthHeaderValue = "Basic {0}" -f $base64EncodedCredentials;

$headers = @{
	Authorization = $basicAuthHeaderValue;
	"Content-Type" = "application/json";
};

$body = @{ UserId = $userId } | ConvertTo-Json;

Invoke-RestMethod -Method Post -Uri "http://appclusive/api/Core/Authentications/BasicLogin" -Headers $headers;
Invoke-RestMethod -Method Post -Uri "http://appclusive/api/Core/Authentications/Impersonate" -Headers $headers -Body $body;

# act on behalf of the specified user

Invoke-RestMethod -Method Post -Uri "http://appclusive/api/Core/Authentications/Logout" -Headers $headers;
```
