This document describes the different authentication mechanisms to access the REST Api.


# Basic

*Code sample*

```powershell
$basicCredentials = "USERNAME:PASSWORD";
$base64EncodedCredentials = $basicCredentials | ConvertTo-Base64;
$basicAuthHeaderValue = "Basic {0}" -f $base64EncodedCredentials;

$headers = @{
	Authorization = $basicAuthHeaderValue;
};

Invoke-RestMethod -Method Post -Uri "http://appclusive/api/Core/Authentications/BasicLogin" -Headers $headers;
Invoke-RestMethod -Method Get -Uri "http://appclusive/api/Core/Users" -Headers $headers;
Invoke-RestMethod -Method Post -Uri "http://appclusive/api/Core/Authentications/Logout" -Headers $headers;
```

# Negotiate

*Code sample*

```powershell
Invoke-RestMethod -Method Post -Uri "http://appclusive/api/Core/Authentications/NegotiateLogin" -UseDefaultCredentials;
Invoke-RestMethod -Method Get -Uri "http://appclusive/api/Core/Users" -UseDefaultCredentials;
Invoke-RestMethod -Method Post -Uri "http://appclusive/api/Core/Authentications/Logout" -UseDefaultCredentials;
```

# OAuth2

## Request authorization code

To get an authorization code perform the following steps:

1. Go to `https://login.microsoftonline.com/d-fens.net/oauth2/authorize?client_id=CLIENT_ID&response_type=code&redirect_uri=http%3A%2F%2Fappclusive%2FCore%2FUsers&response_mode=query`
2. Log in with your microsoft account
3. Copy the authorization code from the URI you got redirected to


## Request access token using authorization code

```
$headers = @{
	"Content-Type" = "application/x-www-form-urlencoded"
};

$authorizationCode = 'AUTHORIZATION_CODE_FROM_URI';

$body = "grant_type=authorization_code&client_id=CLIENT_ID&code={0}&redirect_uri=http%3A%2F%2Fappclusive%2FCore%2FUsers&resource=CLIENT_ID
&client_secret=CLIENT_SECRET" -f $authorizationCode;

Invoke-RestMethod -Method Post -Uri "https://login.windows.net/d-fens.net/oauth2/token" -Body $body -Headers $headers;
```

## Call API with Bearer access token

```
$jwt = "BEARER_TOKEN;
$bearerAuthHeader = "Bearer {0}" -f $jwt;

$headers = @{
	Authorization = $bearerAuthHeader;
};

Invoke-RestMethod -Method Post -Uri "http://appclusive/api/Core/Authentications/BearerLogin" -Headers $headers;
Invoke-RestMethod -Method Get -Uri "http://appclusive/api/Core/Users" -Headers $headers;
Invoke-RestMethod -Method Post -Uri "http://appclusive/api/Core/Authentications/Logout" -Headers $headers;
```
