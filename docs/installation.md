# Installation Guide

This guide will help you install Steev on your system.

## System Requirements

- Python 3.11 or higher
- Packages to execute your training code (e.g. transformers, unsloth)

## Installation Steps

Note that we've tested only on Linux. MacOS and Windows are not supported yet.

### Using pip

The easiest way to install Steev is using pip:

```bash
pip install steev
```

## Verifying Installation

To verify that Steev is installed correctly:

```bash
steev version
```

!!! example "Expected output"
    ```
    steev version 0.1.0 
    ```

## Sign-up / Login

After installation, you need to sign-up or login to your account.

```bash
steev auth login
```

If you are using a remote server, it means no-browser login is required. You can use the auth token to login.

!!! example "Expected output"
    ```
    Choose authentication method:
      Log in with web browser(Login from local)
      Paste an auth token(Login from remote server)
    ```

## Next Steps

Now that you have Steev installed, check out the [Quick Tour](quick-tour.md) to get started! 