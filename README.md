[![ReadyStack](https://readystack.dev/logo.svg)](https://readystack.dev)

**Stable, Open-source Releases, Automated** · [readystack.dev](https://readystack.dev)

[![Docker Hub](https://img.shields.io/badge/Docker%20Hub-readystack%2Fotelcol-2496ED?logo=docker&logoColor=white)](https://hub.docker.com/r/readystack/otelcol) [![Docker pulls](https://img.shields.io/docker/pulls/readystack/otelcol)](https://hub.docker.com/r/readystack/otelcol) [![Grade at release](https://img.shields.io/badge/grade%20at%20release-A%20%2894%25%29-2ea44f)](https://readystack.dev/products/otelcol) [![Signed: cosign](https://img.shields.io/badge/signed-cosign-2ea44f)](https://readystack.dev/keys/cosign.pub) ![SBOM: SPDX](https://img.shields.io/badge/SBOM-SPDX-2ea44f) ![Provenance: SLSA v1](https://img.shields.io/badge/provenance-SLSA%20v1-2ea44f) ![Offline rebuild: verified](https://img.shields.io/badge/offline%20rebuild-verified-2ea44f)

# OpenTelemetry Collector: ReadyStack build kit

The offline build kit for the Docker image `readystack/otelcol:v0.165.0-CE-ubuntu24.04-r3`. It holds what the image is built from (upstream source, base images, toolchain, pinned dependencies) and the documents, so the image can be rebuilt with no network access (`docker build --network=none`).

- Release: [v0.165.0-CE-ubuntu24.04-r3](https://github.com/readystack-dev/otelcol/releases/tag/v0.165.0-CE-ubuntu24.04-r3)
- Image: [`docker.io/readystack/otelcol:v0.165.0-CE-ubuntu24.04-r3`](https://hub.docker.com/r/readystack/otelcol/tags), manifest digest `sha256:c1d25f30f0f551220ae488008fdc7284f16a56dc427211d6c11bf7664890f48a`
- Product page: https://readystack.dev/products/otelcol
- Source and build recipe: https://readystack.dev/products/otelcol/recipe
- Build trail: https://readystack.dev/products/otelcol/trail/v0.165.0-CE-ubuntu24.04-r3
- Defects: this repository's Issues (defects recorded from 25 November 2026 on) and https://readystack.dev/defects.html?product=otelcol

## The image

Single container, single static Go binary (`otelcol --config /etc/otelcol/config.yaml`) in the foreground. A stateless telemetry pipeline: it receives traces, metrics and logs, processes them and forwards them to the backends you configure. It stores nothing itself: no database, no web UI, no query API.

## Who uses OpenTelemetry Collector and why

Platform and observability teams who collect traces, metrics and logs from many applications and send them to backends.

The OpenTelemetry Collector receives, processes and exports telemetry data in a vendor-agnostic way, so one agent or gateway can replace several separate collectors and agents.

A typical use: A team's services emit traces and metrics in different formats, and the team is weighing a new monitoring backend. Every service sends its data to an OpenTelemetry Collector, which processes it and exports it to the current backend and the trial one at the same time, with no change to the services.

From the upstream project's own description: https://github.com/open-telemetry/opentelemetry-collector, https://opentelemetry.io/docs/collector/.

## This release

- Docker Scout policy grade at release: **A** (94%), graded 2026-11-22 with `docker scout policy` (CLI 1.26.0, docker-default configuration).
  - Docker Scout counts 7 packages with copyleft licences (GPL, LGPL, MPL), all of them operating-system packages (it counts only packages whose licence it can read; the SBOM lists every component).
- Signed with ReadyStack's release key (fingerprint `04daa92344a52dbc`): https://readystack.dev/keys/cosign.pub.
- Runs as a non-root user (`otelcol`).
- Attestations on the image: an SPDX SBOM and SLSA provenance v1.
- Rebuilt from this kit with network egress blocked on 2026-11-22; the rebuilt image digest is the published one.
- Base: Ubuntu 24.04.

The current grade, measured again every day, is on https://readystack.dev/products/otelcol.

OpenTelemetry Collector is distributed under its upstream licence (Apache-2.0); the kit contains upstream's source under that licence, and each vendored component under its own.

## Download

From the [v0.165.0-CE-ubuntu24.04-r3 release](https://github.com/readystack-dev/otelcol/releases/tag/v0.165.0-CE-ubuntu24.04-r3): `ReadyStack-otelcol-v0.165.0-CE-ubuntu24.04-r3.tar.gz` (457.6 MB), SHA-256 `6c30c945c3cd26ab7911329011f2388520ae560bc6bddf3d46884633afaf2001`.

```sh
sha256sum --check --ignore-missing SHA256SUMS
```

## Verify and rebuild

The rebuild statement is signed with the ReadyStack release key. Check the signature before you rely on it:

```sh
cosign verify-blob --key https://readystack.dev/keys/cosign.pub \
  --bundle ReadyStack-otelcol-v0.165.0-CE-ubuntu24.04-r3.rebuild.json.sigstore ReadyStack-otelcol-v0.165.0-CE-ubuntu24.04-r3.rebuild.json
```

Unpack with `-p` so file modes are kept, then follow the kit's own README.md and docs/REBUILD.md:

```sh
tar -xpzf ReadyStack-otelcol-v0.165.0-CE-ubuntu24.04-r3.tar.gz
cd v0.165.0-CE-ubuntu24.04-r3
less README.md docs/REBUILD.md
```

The build trail (`ReadyStack-otelcol-v0.165.0-CE-ubuntu24.04-r3.trail.json`, signed the same way, and its Markdown copy) records how this release was built and checked.

## This repository

`recipe/` holds the kit's recipe files (Dockerfiles, build scripts, pins) as published at https://readystack.dev/products/otelcol/recipe. The complete kit is the release download. Each release is a tag here. How to report a vulnerability: [SECURITY.md](SECURITY.md).

## About ReadyStack

**Stable, Open-source Releases, Automated.** ReadyStack releases popular open-source apps every week with every available critical and high security fix applied, plus full documentation, reference data, tests and a build recipe that recreates each release bit-for-bit. Each release is rebuilt from its pinned inputs with the network off, checked against its published digest, signed, and scanned and graded again every day. The images and build kits are free, and ReadyStack is soon to be a nonprofit.

[readystack.dev](https://readystack.dev) · [All products](https://readystack.dev/products.html) · [Build kits on GitHub](https://github.com/readystack-dev) · [Defect log](https://readystack.dev/defects.html) · contact@readystack.dev

— ReadyStack · https://readystack.dev
