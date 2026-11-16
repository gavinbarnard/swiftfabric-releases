# SwiftFabric releases

This repository contains downloadable SwiftFabric release bundles. It is a
release-only repository: source code and build tooling live elsewhere. Each
`.tar.gz` bundle includes prebuilt Debian packages, installation and upgrade
tools, release metadata, and test reports.

## Available releases

| Release | Bundle | Supported upgrade from |
| --- | --- | --- |
| `alpha_rc2` | [`swiftfabric-alpha_rc2.tar.gz`](./swiftfabric-alpha_rc2.tar.gz) | `alpha_rc1` |
| `alpha_rc1` | [`swiftfabric-alpha_rc1.tar.gz`](./alpha/swiftfabric-alpha_rc1.tar.gz) | `alpha_rc0` |
| `alpha_rc0` | [`swiftfabric-alpha_rc0.tar.gz`](./alpha/swiftfabric-alpha_rc0.tar.gz) | None declared |

Each bundle has a matching `.sha256` file. See the bundle's `RELEASE-NOTES.md`
and `RELEASE.json` for its source revisions, package versions, test results,
requirements, and upgrade policy.

## Verify and unpack

From this directory, verify the latest release archive before extracting it:

```sh
sha256sum --check swiftfabric-alpha_rc2.tar.gz.sha256
tar -xzf swiftfabric-alpha_rc2.tar.gz
cd swiftfabric-alpha_rc2
sha256sum --check SHA256SUMS
```

The first checksum verifies the archive; the bundled `SHA256SUMS` verifies its
contents.

## Install

Review `RELEASE-NOTES.md` and `TROUBLESHOOTING.md` first. The installer requires
Python 3 with `venv`, network access to fetch the Ubuntu cloud image and
installer dependencies, and SSH/API access to the target Proxmox cluster.

```sh
./pull-images.sh
cp config.example.json config.json
# Edit config.json for your environment.
./install.sh --non-interactive --config config.json
```

Protect `config.json` and any credentials it contains; do not publish them.

## Upgrades

`alpha_rc2` supports upgrading an existing `alpha_rc1` deployment. This path
passed upgrade acceptance and ordered cleanup; the bundle includes before- and
after-upgrade reports. The previous `alpha_rc1` release supports upgrading
from `alpha_rc0`. No other starting release is declared as supported.

From the verified, extracted `alpha_rc2` bundle, use the original deployment
configuration and securely enter the current cluster bearer token:

```sh
./upgrade.sh --config /path/to/original/config.json --token
```

This upgrades controller packages while preserving controller state. Compute
hosts and North/South router agents require separate user-initiated upgrades,
unless `--auto-agent-upgrade` is supplied. Review the bundled `RELEASE-NOTES.md`,
`RELEASE.json`, and `BUILDING.md` before upgrading.

Treat other upgrade paths as potentially breaking until explicitly declared
supported. A breaking change may require destroying and recreating the
deployment; that loses controller state.
