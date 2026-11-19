# Fluyt alpha binaries

This repository distributes prebuilt Fluyt alpha executables. Fluyt runs an
HTTPS management server with its UI embedded; Go, Node.js and a separate database
server are not required to run it.

[Browse prereleases](https://github.com/fluytnet/fluyt_public/releases).
Automated candidates are published as `v0.0.0-alpha.<run number>`. A prerelease
may be used by the demo before it is recommended for general evaluation.
**No recommended release has been selected yet.** Alpha releases are for
controlled evaluation and carry no stable-version or general vendor-support claim.

| Platform | Asset |
| --- | --- |
| Linux x86-64 | `fluyt-linux-amd64` |
| macOS Apple silicon | `fluyt-darwin-arm64` |
| Windows x86-64 | `fluyt-windows-amd64.exe` |

Download your executable, `SHA256SUMS` and `repository-sbom.spdx.json` from the
same exact tag. Release notes identify the original private build commit and
the separate distribution commit in this repository. This repository contains
distribution guidance, not the product source.

## Verify a download

Use a GitHub CLI version with immutable release verification. Replace the
example tag with the exact release you selected:

```sh
tag=v0.0.0-alpha.42
gh release verify "$tag" --repo fluytnet/fluyt_public
gh release verify-asset "$tag" ./fluyt-linux-amd64 --repo fluytnet/fluyt_public
gh release verify-asset "$tag" ./SHA256SUMS --repo fluytnet/fluyt_public
gh release verify-asset "$tag" ./repository-sbom.spdx.json --repo fluytnet/fluyt_public
```

Use your platform's executable filename. Run `verify-asset` for every downloaded
file. On Windows, use PowerShell variables such as `$tag = 'v0.0.0-alpha.42'`.
The manifest records SHA-256 hashes for the three executables and SBOM;
`sha256sum --ignore-missing --check SHA256SUMS` provides an additional local
comparison on Linux. Checksums alone do not establish who published the files.

The SBOM is GitHub's native repository dependency-graph snapshot exported during
release preparation. It may be older or newer than the build commit. It is not
an exact dependency inventory for each compiled executable. Immutable release
verification proves the published tag/assets; the build-commit label is the
publisher's assertion, backed by qualification in the private source repository.

## First start

Run the executable under your ordinary OS account with a dedicated absolute data
directory and an operator-provided TLS certificate and private key:

```sh
chmod +x ./fluyt-linux-amd64
./fluyt-linux-amd64 --data-dir /absolute/private/fluyt \
  --tls-cert /absolute/path/localhost.pem --tls-key /absolute/path/localhost-key.pem
```

On macOS use `./fluyt-darwin-arm64`; on Windows use
`.\fluyt-windows-amd64.exe` with absolute Windows paths. Open the HTTPS address
printed by the server and complete first-run setup. Protect the TLS private key
and data directory. The initial inventory is empty. Configure devices,
authentication, TLS trust and licensing before expecting live device data.
Telemetry and change execution are separate subsystems that require operator
intent to start.

Device credentials use an externally provisioned JSON file with `username` and
`password`. Select its absolute path through Fluyt's authentication UI. Private
permissions are recommended; readable shared files carry a protection warning.
Rotate to a new filename/version rather than edit an active credential document.
Windows paths must use their canonical local-drive spelling; network/device
namespaces, short-name aliases, reparse points and hard links are unsupported.

A signed license is required for licensed capabilities; public download does
not issue a license or grant product-source rights. Obtain evaluation access
and applicable terms from the Fluyt owner. Do not commit credentials, licenses,
installation identity, device inventories or private keys to this repository.

Keep data and executable backups separately. Stop the server before replacing
an executable, then restart with the same account and data directory. Fresh
alpha databases are the support policy; do not assume upgrade/migration support.

Linux release checks exercise the executable through isolated HTTPS setup,
login, inventory streaming and restart using simulated device transport.
Native platform credential/runtime and SQLite checks have their own CI scope.
These checks do not qualify every live vendor, device version or feature.
