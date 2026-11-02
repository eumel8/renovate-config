# renovate-config

Shared Renovate presets for eumel8, eumelnet and mcsps.

## Use in a repository

Install the [Renovate GitHub App](https://github.com/apps/renovate) for the repository,
then add `renovate.json`:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>eumel8/renovate-config"]
}
```

Merge the initial preset PR in this repository before merging consumer PRs.
Remove overlapping Dependabot version-update entries when switching a repository.
GitHub vulnerability alerts and Dependabot security updates are separate settings;
this preset does not disable them or close existing Dependabot PRs.

## Update policy

- New update branches and scheduled branch updates: Monday, 00:00–08:00 Europe/Berlin.
- At most three open regular Renovate PRs per repository; security updates can bypass limits.
- Non-major updates grouped into Go modules, Go toolchain, GitHub Actions and container images.
- Go Docker images, Go archive URLs in workflows and `go.mod` Go/toolchain directives
  share the Go toolchain group. Grouping does not repair pre-existing version drift.
- The minimum Go version in `go.mod` is deliberately updated for these application repositories.
- Major upgrades require approval through the repository's Dependency Dashboard.
- Auto-merge is disabled.

The schedule allows Renovate to work during that window; it does not schedule the
bot itself. App installation and completed onboarding are required. Changes to
`default.json` on the default branch affect all repositories extending this preset.

Libraries that must preserve their minimum Go version can override that rule:

```json
{
  "extends": ["github>eumel8/renovate-config"],
  "packageRules": [
    {
      "matchManagers": ["gomod"],
      "matchDepTypes": ["golang"],
      "enabled": false
    }
  ]
}
```

## Downloaded tools

The preset includes Renovate's standard Dockerfile and workflow `_VERSION`
custom managers. Annotate pinned downloads, for example:

```dockerfile
# renovate: datasource=github-tags depName=kubernetes/kubernetes
ARG KUBECTL_VERSION=v1.36.0
```

Use `${KUBECTL_VERSION}` in the download URL. Apply repository-specific version
limits for tools that must stay compatible with a particular Kubernetes release.
Unpinned package installations and floating tags such as `alpine:latest` do not
produce version-number updates; they still need image rebuilds or digest pinning.

## Validation and references

GitHub Actions validates `default.json` with the Renovate config validator.

- [Shared presets](https://docs.renovatebot.com/config-presets/)
- [Custom manager presets](https://docs.renovatebot.com/presets-customManagers/)
- [Go directives](https://docs.renovatebot.com/modules/manager/gomod/)
- [Configuration options](https://docs.renovatebot.com/configuration-options/)
