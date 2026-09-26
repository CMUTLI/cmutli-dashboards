# CMU TLI Dashboards

This repository holds the dashboards and provisioning files for
`ops.tli.cmu.edu`. `cmutli-fleet` owns the server, collector, container,
and deployment. `cmutli-fleet-secrets` owns credentials. Grafana is the current
dashboard application; its files live under `grafana/`.

## Development

Enter the development shell, then check a dashboard and the provisioning files
before committing them.

```console
nix develop
jq empty grafana/dashboards/<dashboard>.json
yamllint grafana/provisioning
```

Add a stable `uid` to each dashboard JSON file. Keep credentials and API tokens
out of dashboard exports and provisioning files.

## Deployment

The fleet pins this repository as a flake input and mounts `grafana/` read-only
into the Grafana container. After pushing dashboard changes, update the input
in `cmutli-fleet` and deploy `ops-01.tli.cmu.edu`.
