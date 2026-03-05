---
name: setup-gws
description: Install and configure the gws CLI for Gmail access.
---

# Setup gws for Gmail

Install the Google Workspace CLI, authenticate, and enable the Gmail API.

## Step 1 — Check if gws is installed

```bash
which gws
```

If the command is not found, install it:

```bash
npm install -g @googleworkspace/cli
```

Or build from source with Cargo:

```bash
cargo install --path .
```

## Step 2 — Authenticate

### First-time setup (recommended)

This walks you through GCP project creation, API enablement, and OAuth login in one step:

```bash
gws auth setup
```

### Login only (if project is already configured)

```bash
gws auth login
```

### Multiple accounts

```bash
gws auth login --account work@company.com
gws auth login --account personal@gmail.com
gws auth list                                # see registered accounts
gws auth default work@company.com            # set default
```

## Step 3 — Verify Gmail access

```bash
gws gmail users getProfile --params '{"userId": "me"}'
```

A successful response returns your email address and message counts. If you get an `accessNotConfigured` error, enable the Gmail API:

1. Open the URL from the error message (or go to the GCP Console > APIs & Services > Library).
2. Search for "Gmail API" and click **Enable**.
3. Wait a few seconds and retry.

## Step 4 — Verify auth status anytime

```bash
gws auth list
```

This shows all registered accounts and which one is active. If the list is empty or the desired account isn't shown, run `gws auth login` again.

## Headless / CI environments

For servers without a browser:

1. On a machine with a browser, complete `gws auth login` then export:
   ```bash
   gws auth export --unmasked > credentials.json
   ```
2. On the headless machine:
   ```bash
   export GOOGLE_WORKSPACE_CLI_CREDENTIALS_FILE=/path/to/credentials.json
   ```

## Service account

```bash
export GOOGLE_WORKSPACE_CLI_CREDENTIALS_FILE=/path/to/service-account.json
export GOOGLE_WORKSPACE_CLI_IMPERSONATED_USER=admin@company.com  # for domain-wide delegation
```

## Troubleshooting

| Problem | Fix |
|---------|-----|
| `command not found: gws` | Run `npm install -g @googleworkspace/cli` |
| `accessNotConfigured` | Enable the Gmail API in your GCP project (link in error) |
| Token expired | Run `gws auth login` to refresh |
| Wrong account | Run `gws auth list` and `gws auth default <email>` |
