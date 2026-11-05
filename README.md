# SwiftFabric releases

This repository contains downloadable SwiftFabric release bundles. It is a
release-only repository: source code and build tooling live elsewhere. Each
`.tar.gz` bundle includes prebuilt Debian packages, installation and upgrade
tools, release metadata, and test reports.

## Available releases

| Release | Bundle |
| --- | --- |
| `alpha_rc0` | [`swiftfabric-alpha_rc0.tar.gz`](./swiftfabric-alpha_rc0.tar.gz) |

Each bundle has a matching `.sha256` file. See the bundle's `RELEASE-NOTES.md`
and `RELEASE.json` for its source revisions, package versions, test results,
requirements, and upgrade policy.

## Verify and unpack

From a checkout of this repository, verify the downloaded archive before
extracting it:

```sh
sha256sum --check swiftfabric-alpha_rc0.tar.gz.sha256
tar -xzf swiftfabric-alpha_rc0.tar.gz
cd swiftfabric-alpha_rc0
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

Treat upgrades between releases as potentially breaking until a supported
starting release is declared in that release's metadata and notes. Read those
notes before using `upgrade.sh`. A breaking change may require destroying and
recreating the deployment; that loses controller state.
