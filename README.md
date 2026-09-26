# CMU TLI Dashboards

This repository holds the dashboards and provisioning files served by
`ops.tli.cmu.edu` for the CMU TLI fleet. The public `cmutli-fleet` repository
contains the server, collector, container, and deployment; `cmutli-fleet-secrets`
contains credentials.

## Background

Grafana is the current dashboard application; its files live under
`grafana/`. The fleet pins this repository as a flake input and mounts
`grafana/` read-only into the Grafana container, so a dashboard change reaches
`ops.tli.cmu.edu` only after the fleet's pin is updated and deployed.

Keep credentials and API tokens out of dashboard exports and provisioning
files.

## Setup

The development shell provides Git, jq, and yamllint.

```console
nix develop
```

## Add or change a dashboard

### In `cmutli-dashboards`

Give each dashboard JSON file a stable `uid`. Check the dashboard and the
provisioning files before committing them.

```console
nix develop
jq empty grafana/dashboards/<dashboard>.json
yamllint grafana/provisioning
git add grafana
git commit -m "feat: add <dashboard> dashboard"
git push
```

### In `cmutli-fleet`

Refresh the pinned dashboards input, then deploy the hosts serving
`ops.tli.cmu.edu`.

```console
cd ../cmutli-fleet
nix develop
nix flake update cmutli-dashboards
git add flake.lock
git commit -m "chore: update dashboards input"
git push
```
