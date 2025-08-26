# DHCore credentials

DHCore credentials control authentication to the DHCore backend. The SDK supports multiple auth methods: personal access token (exchange), OAuth2 (access + refresh tokens), access token only, and basic (username + password). The configurator inspects available keys and selects the appropriate auth flow.

## Get credentials

If credentials are not provided in the environment/configuration file, the user must login interactively to obtain them. To do that, use the the Digitalhub CLI tool. You can find more information in [CLI documentation page](https://scc-digitalhub.github.io/docs/components/cli/).

## Important fields

- DHCORE_ENDPOINT — DHCore backend endpoint (e.g., `https://dhcore.example.com`)
- DHCORE_ISSUER — OpenID issuer (optional; used for OAuth2 flows)
- DHCORE_USER — username (for BASIC auth)
- DHCORE_PASSWORD — password (for BASIC auth)
- DHCORE_CLIENT_ID — OAuth2 client id (used for refresh/exchange)
- DHCORE_ACCESS_TOKEN — access token (bearer)
- DHCORE_REFRESH_TOKEN — refresh token (OAuth2)
- DHCORE_PERSONAL_ACCESS_TOKEN — personal access token (used for token exchange)

!!! warning
    Please note that there are difference in prefixation of variables in environment and file.

## Environment variables

Set the variables in your shell or CI environment. Example:

```bash
export DHCORE_ENDPOINT=`https://dhcore.example.com`

# PAT auth
export DHCORE_PERSONAL_ACCESS_TOKEN=pat_...
export DHCORE_CLIENT_ID=client-id

# Access token + refresh token
export DHCORE_ACCESS_TOKEN=eyJ...
export DHCORE_REFRESH_TOKEN=...
export DHCORE_CLIENT_ID=client-id

# Access token only
export DHCORE_ACCESS_TOKEN=eyJ...

# Basic auth
export DHCORE_USER=myuser
export DHCORE_PASSWORD=mypassword
```

## Configuration file (.dhcore.ini)

Credentials can also live in `.dhcore.ini`. Note the difference in prefix. Example profile:

```text
[__default]
DHCORE_ENDPOINT = `https://dhcore.example.com`

# PAT auth
DHCORE_PERSONAL_ACCESS_TOKEN = pat_...
CLIENT_ID = client-id

# Access token + refresh token
ACCESS_TOKEN = eyJ...
REFRESH_TOKEN = ...
CLIENT_ID = client-id

# Access token only
ACCESS_TOKEN = eyJ...

# Basic auth
DHCORE_USER = myuser
DHCORE_PASSWORD = mypassword
```

## Notes

- The configurator will set the appropriate auth method based on available credentials. It will give precedence to PAT auth, then access token + refresh token, followed by access token only, and finally basic auth. If no credentials are found, no authentication will be performed.
- The configurator selects credentials in priority order and will switch to file-based storage when a personal access token needs to be exchanged for access/refresh tokens.
- For OAuth2 and exchange-based auth the SDK can refresh tokens when supported; store refresh tokens and client id in the file if persistent refresh is required.
