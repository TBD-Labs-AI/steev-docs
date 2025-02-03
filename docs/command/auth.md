# Authentication

Manage your authentication using the `steev auth` command.

```bash
steev auth <subcommand>
```

During the beta phase, Steev is completely free. However, authentication is required since the LLM Agent needs to analyze your logs to provide optimal service.

## Subcommands

### login
Log in or sign up for your Steev account. You have two authentication methods available:

1. Browser-based login (recommended for local machines)
2. Token-based login (useful for remote servers)

```bash
steev auth login
```

When you run the login command, you'll see:
```
Choose authentication method:
> Log in with web browser (Login from local)
  Paste an auth token (Login from remote server)
```

Steev uses Google OAuth for authentication. After successful login, your credentials are stored in `~/.steev/credentials.json`:

```json
{
  "user": "username", 
  "email": "steev@steev.io", 
  "token": {
    "access_token": "your-access-token",
    "refresh_token": "your-refresh-token",
    "access_exp": 1738657951  // Expiration timestamp
  }
}
```

### status
Check your current authentication status:

```bash
steev auth status
```

Example output:
```
Credential
  user   
  email  steev@steev.io
tokens
  access_token   your-access-token...
  refresh_token  your-refresh-token...
  access_exp     2099-12-25 23:59:59
```

### logout
Sign out and remove stored credentials:

```bash
steev auth logout
```

This command:
- Revokes your access and refresh tokens
- Deletes the `~/.steev/credentials.json` file

You'll see:
```
Successfully logged out
```

### verify
Verify if your current credentials are valid:

```bash
steev auth verify
```

You'll see one of these messages:
```
Credential token is valid
```
or
```
Credential token is invalid
```

### refresh
Manually refresh your authentication tokens:

```bash
steev auth refresh
```

This command:
- Obtains new access and refresh tokens
- Updates the `~/.steev/credentials.json` file

You'll see:
```
Credential token refreshed
```
