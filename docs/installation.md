# Installation Guide

This guide will help you install Steev on your system and get started quickly.

## System Requirements

- Python 3.11 or higher
- Packages required for your training code (e.g. transformers, unsloth)

!!! note "Platform Support"
    Currently, Steev has been tested only on Linux systems. MacOS and Windows support is planned for future releases.

## Installation Steps

### Using pip (Recommended)

The easiest way to install Steev is using pip:

```bash
pip install steev
```

### Verifying Installation

After installation, verify that Steev is installed correctly by running:

```bash
steev --version
```

You should see output similar to:
```
steev, version 0.1.0
```

## Authentication

### Initial Setup

Before using Steev, you'll need to authenticate. Run:

```bash
steev auth login
```

You'll be presented with two authentication options:

1. **Browser Login** (Recommended for local machines)
   - Opens your default web browser for a seamless login experience
   
2. **Token Login** (For remote servers)
   - Uses an authentication token when browser access isn't available

```
Choose authentication method:
  Log in with web browser (Login from local)
  Paste an auth token (Login from remote server)
```

!!! tip "Remote Server Usage"
    If you're working on a remote server without browser access, choose the "Paste an auth token" option. You can generate an auth token from your account settings page.

## Next Steps

Now that you have Steev installed and configured:

1. Follow our [Quick Tour](quick-tour.md) to learn the basics
2. Explore the [Command Reference](command/run.md) for detailed usage
3. Check out [Tutorials](tutorials/overview.md) for practical examples

!!! question "Need Help?"
    If you run into any issues during installation, please join our community [Discord](https://discord.gg/UxMXBHUWcr).