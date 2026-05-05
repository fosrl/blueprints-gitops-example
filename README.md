# Pangolin Blueprints CI Example

This is a small example repository for applying Pangolin blueprints from GitHub Actions.

It is intentionally minimal: the sample blueprints are dummy environment definitions, and the workflow shows the non-interactive CLI shape needed for CI.

## Layout

```text
.
├── blueprints/
│   ├── customer-a.yaml
│   └── customer-b.yaml
└── .github/
    └── workflows/
        └── apply-blueprints.yml
```

## How It Works

Blueprint files live under [blueprints](./blueprints). Each file can define private resources, public resources, targets, sites, and access settings for a Pangolin environment.

When changes are pushed to `main`, GitHub Actions:

1. checks out the repository
2. installs the Pangolin CLI
3. applies every `*.yaml` file in `./blueprints`

See [.github/workflows/apply-blueprints.yml](./.github/workflows/apply-blueprints.yml) for the full workflow.

## Required Secrets

Add these repository secrets in GitHub before running the workflow:

- `PANGOLIN_INTEGRATION_API_KEY`
- `PANGOLIN_ENDPOINT`
- `PANGOLIN_ORG_ID`

`PANGOLIN_ENDPOINT` should be the Pangolin Integration API endpoint, not the dashboard URL. For a self-hosted deployment this is typically the externally reachable API base URL, for example `https://api.example.com/v1`.

References:

- [Integration API](https://docs.pangolin.net/manage/integration-api)
- [Enable Integration API for self-hosted Pangolin](https://docs.pangolin.net/self-host/advanced/integration-api)

The workflow passes those values to:

```bash
pangolin apply blueprint \
  --api-key "$PANGOLIN_INTEGRATION_API_KEY" \
  --endpoint "$PANGOLIN_ENDPOINT" \
  --org "$PANGOLIN_ORG_ID" \
  --file "$blueprint"
```

## Adding A Blueprint

Create another YAML file in `blueprints/`, for example:

```text
blueprints/customer-c.yaml
```

On the next push to `main`, the workflow will include it automatically.

## Notes

- The domains, users, roles, and targets in this repo are examples only.
- Do not commit Pangolin credentials or private deployment details.
- Review blueprint changes like any other access or exposure change.
