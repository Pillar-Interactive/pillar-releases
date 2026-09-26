# Pillar releases

Signed release artifacts for the Pillar agent and `pillar` CLI. This repository
contains **binaries and signatures only**. Pillar's source code is not published
here, and nothing in this repository grants a licence to it.

Releases are published automatically by Pillar's release pipeline. Do not open
pull requests or push tags here.

## Installing

Install using the command the Pillar dashboard generates for your organization.
It downloads and verifies these files for you.

## Release assets

| File | Purpose |
| --- | --- |
| `agent-release.env` | Signed manifest: agent and observer image digests, CLI version and checksums |
| `pillar-linux-amd64`, `pillar-linux-arm64` | `pillar` CLI (static Linux binaries) |
| `pillar-agent-update` | Host updater script |
| `*.sigstore.json` | Sigstore bundle for the file of the same name |

Container images are published separately at `ghcr.io/pillar-interactive/pillar-agent`
and `ghcr.io/pillar-interactive/pillar-observer`, pinned by digest in `agent-release.env`.

## Verifying a download

Every asset is signed keylessly with [Sigstore](https://www.sigstore.dev/) by
Pillar's release workflow. Verify with [cosign](https://github.com/sigstore/cosign) v3:

```bash
cosign verify-blob \
  --bundle agent-release.env.sigstore.json \
  --certificate-identity-regexp '^https://github\.com/Pillar-Interactive/pillar/\.github/workflows/publish-agent\.yml@refs/tags/v[0-9]+\.[0-9]+\.[0-9]+$' \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com \
  agent-release.env
```

Then check the CLI binary against the manifest:

```bash
grep "cli_linux_amd64_sha256" agent-release.env
sha256sum pillar-linux-amd64
```

## Support

support@pillarinteractive.com

Copyright © Pillar Interactive LLC. All rights reserved.
