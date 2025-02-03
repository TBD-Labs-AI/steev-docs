# Authentication

Manage your authentication using the `steev auth` command.

```bash
steev auth <subcommand>
```

This command allows you to authenticate your Steev account. You can sign in or log in using this command. During the beta phase, Steev is completely free. However, authentication is required because the LLM Agent needs to analyze your logs to provide optimal service.

## Subcommands

### login

```bash
steev auth login
```

Use this command to log in or sign up for your Steev account. You have two login methods to choose from:

1. Open a web browser
2. Use the CLI

!!! example "Expected output"
    ```
    # Login prompt
    Choose authentication method:
    > Log in with web browser (Login from local)
      Paste an auth token (Login from remote server)
    ```

If you are using a remote server, you can log in by pasting an auth token.
`steev` uses Google OAuth for authentication.

Once you log in, your token will be saved in the `~/.steev/credentials.json` file.

!!! example "Expected output"
    example of `credentials.json` file:
    ```json
    {
      "user": "username", 
      "email": "steev@steev.io", 
      "token": {
        "access_token": "some access token",
        "refresh_token": "some refresh token",
        "access_exp": 1738657951 # access token expiration time in integer
      }
    }
    ```

### status

You can check your credentials using the `status` subcommand.

```bash
steev auth status
```

!!! example "Expected output"
    ```
    Credential
      user   
      email  steev@steev.io
    tokens
      access_token   some access token...
      refresh_token  some refresh token...
      access_exp     2099-12-25 23:59:59
    ```

### logout

You can log out using the `logout` subcommand.

```bash
steev auth logout
```

!!! example "Expected output"
    ```prompt
    Successfully logged out
    ```

This commands revoke your access token and refresh token. and delete the `~/.steev/credentials.json` file.


### verify

You can verify your credentials using the `verify` subcommand.

```bash
steev auth verify
```

If the credentials are valid, you will see the following output:
!!! example "Expected output"
      ```prompt
      Credential token is valid
      ```

If the credentials are invalid, you will see the following output:
!!! example "Expected output"
      ```prompt
      Credential token is invalid
      ```


### refresh

You can refresh your credentials using the `refresh` subcommand.

```bash
steev auth refresh
```

!!! example "Expected output"
      ```prompt
      Credential token refreshed
      ```

This commands refresh your access token and refresh token. and update the `~/.steev/credentials.json` file.



