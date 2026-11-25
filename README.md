# OpenTelemetry Collector: ReadyStack build kit

The offline build kit for the Docker image `readystack/otelcol:v0.165.0-CE-ubuntu24.04-r3`. It holds what the image is built from (upstream source, base images, toolchain, pinned dependencies) and the documents, so the image can be rebuilt with no network access (`docker build --network=none`).

- Release: [v0.165.0-CE-ubuntu24.04-r3](https://github.com/readystack-dev/otelcol/releases/tag/v0.165.0-CE-ubuntu24.04-r3)
- Image: [`docker.io/readystack/otelcol:v0.165.0-CE-ubuntu24.04-r3`](https://hub.docker.com/r/readystack/otelcol/tags), manifest digest `sha256:c1d25f30f0f551220ae488008fdc7284f16a56dc427211d6c11bf7664890f48a`
- Product page: https://readystack.dev/products/otelcol
- Defects: this repository's Issues (defects recorded from 25 November 2026 on) and https://readystack.dev/defects.html?product=otelcol

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

`recipe/` holds the kit's recipe files (Dockerfiles, build scripts, pins) as published at https://readystack.dev/products/otelcol/recipe. The complete kit is the release download. Each release is a tag here.
