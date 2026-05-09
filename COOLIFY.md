# Deploy 9Router on Coolify

This fork can be deployed directly from Coolify with the included Dockerfile.

## Coolify resource

1. Create a new resource in Coolify.
2. Select this GitHub repository.
3. Use `Dockerfile` as the build pack.
4. Set the exposed/internal port to `20128`.
5. Add a persistent volume mounted at:

```text
/app/data
```

6. Attach your domain, for example:

```text
https://router.example.com
```

## Environment variables

Set these in Coolify. Do not commit real values to the repository.

```env
JWT_SECRET=replace-with-a-long-random-secret
INITIAL_PASSWORD=replace-with-your-dashboard-password
DATA_DIR=/app/data
PORT=20128
HOSTNAME=0.0.0.0
NODE_ENV=production

BASE_URL=https://router.example.com
NEXT_PUBLIC_BASE_URL=https://router.example.com
CLOUD_URL=https://9router.com
NEXT_PUBLIC_CLOUD_URL=https://9router.com

API_KEY_SECRET=replace-with-another-long-random-secret
MACHINE_ID_SALT=replace-with-another-long-random-secret
AUTH_COOKIE_SECURE=true
REQUIRE_API_KEY=true
ENABLE_REQUEST_LOGS=false
OBSERVABILITY_ENABLED=true
```

Generate secrets on your VPS with:

```bash
openssl rand -hex 32
```

Use a different value for `JWT_SECRET`, `API_KEY_SECRET`, and `MACHINE_ID_SALT`.

## After deploy

Open:

```text
https://router.example.com/dashboard
```

Log in with `INITIAL_PASSWORD`, connect providers in the dashboard, then copy the generated API key.

Use 9Router from tools with:

```text
Endpoint: https://router.example.com/v1
API Key: the key copied from the dashboard
Model: the model/provider configured in 9Router
```

## Security notes

- Keep port `20128` private behind Coolify's HTTPS reverse proxy when possible.
- Keep `AUTH_COOKIE_SECURE=true` when using HTTPS.
- Keep `REQUIRE_API_KEY=true` for internet-exposed deployments.
- Never commit `.env` or real provider credentials.
