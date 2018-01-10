# General

| Problem description | Solution |
|:-------------|:-----|
| HTTP 400 - Bad Request (Request header too long) | [Microsoft Knowledge Base 2020943](https://support.microsoft.com/en-us/kb/2020943) / Check, if there is a space in the name of the registry key |
| HTTP 503 | Check status of Application Pool and start it, if stopped |
|  | Check, if password of Application Pool user is expired (Reset if necessary) |
| `No connection could be made because the target machine actively refused it 127.0.0.1:8888` | Run and exit Fiddler |
| User cannot consent to web app requesting user impersonation as an app permission | Change delegated permission `Access the directory as the signed-in user` to `Sign in and read user profile` for Windows Azure Active Directory application |
