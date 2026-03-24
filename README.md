# Keycloak + RAGFlow OIDC test stack

This directory is a self-contained copy of `ragflow/docker` with only local changes for Keycloak-based OIDC testing.

## Layout

- `docker/`: copied from `ragflow/docker`
- `docker-compose.keycloak.yml`: root compose file that includes the copied RAGFlow stack and adds Keycloak
- `docker/keycloak-realm.json`: imported Keycloak realm with a pre-created RAGFlow OIDC client and test user

## Defaults

- RAGFlow UI: `http://localhost:18080`
- Keycloak: `http://127.0.0.1.sslip.io:18088`
- Keycloak admin: `admin / admin1234`
- Keycloak test user: `ragtester / ragtester123!`
- RAGFlow OIDC client: `ragflow`

## Run

```bash
cd /home/yrshin/projects/keycloak-rag-test
docker compose -f docker-compose.keycloak.yml --env-file ./docker/.env up -d
```

Then open `http://localhost:18080` and use the `Keycloak` login button.

## Notes

- The RAGFlow callback URI is configured as `http://localhost:18080/v1/user/oauth/callback/keycloak`.
- The OIDC issuer uses `127.0.0.1.sslip.io` so the browser and the RAGFlow container can both reach Keycloak without editing `/etc/hosts`.
- If `sslip.io` name resolution is unavailable in your environment, change `KEYCLOAK_PUBLIC_HOST` in `docker/.env` to a hostname you control and map that name to `127.0.0.1` on the host.
