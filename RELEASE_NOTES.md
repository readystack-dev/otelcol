# OpenTelemetry Collector v0.165.0 — ReadyStack release `v0.165.0-CE-ubuntu24.04-r3`

Generated 2026-11-22 from this release's build records and strategy. Statements that come from a single record name it in brackets; the others come from this release's strategy and build log.

## Release

- **Image:** `readystack/otelcol:v0.165.0-CE-ubuntu24.04-r3` (linux/amd64). Index digest `sha256:ba73b28251588975942fdc07d13ed79c046c4e43a30d4244a8087458c5a1604f` [`docs/provenance/LOCKED_DIGEST.txt`].
- **Image manifest digest:** `sha256:c1d25f30f0f551220ae488008fdc7284f16a56dc427211d6c11bf7664890f48a` [`TARGET_DIGEST.txt`].
- **Upstream:** OpenTelemetry Collector v0.165.0 built from source at `v0.165.0` of https://github.com/open-telemetry/opentelemetry-collector-releases — upstream notes: https://github.com/open-telemetry/opentelemetry-collector-releases/releases/tag/v0.165.0 [`docs/provenance/BUILD_RECIPE.json`].
- **Base image:** none (`FROM scratch`): a chiselled Ubuntu 24.04 runtime, the files Canonical's chisel cut from Ubuntu's own packages (`vendored/chisel/slices.txt`, `vendored/chisel/packages.tsv`); runtime user `otelcol`.
- **Build:** `source-rebuild-go`, compiled offline with `--network=none`.
- **Supersedes:** ReadyStack release `v0.165.0-CE-ubuntu24.04-r2`.

## What changed in this release

- Upstream version unchanged (v0.165.0); ReadyStack rebuild.
- Minimum dependency versions ReadyStack enforces on top of upstream (raised only where upstream's own version was lower); shipped versions from the SBOM [`docs/sbom/SBOM.json`]:
  - `golang.org/x/net` at least `v0.62.0` (shipped: `v0.62.0`)
  - `github.com/klauspost/compress` at least `v1.18.7` (shipped: `v1.20.3`)
- OpenTelemetry Collector v0.165.0 is unchanged from the release this supersedes (v0.165.0-CE-ubuntu24.04-r2); this release rebuilds it. Changes: the image is rebuilt on the current ubuntu:24.04 image (sha256:9c7f200c07e3…), which its maintainers updated after v0.165.0-CE-ubuntu24.04-r2 was built on sha256:2198e4c65076…. Settings, ports and data volumes are those of v0.165.0-CE-ubuntu24.04-r2: replace the container with this image on the same volumes.

## Security status at release (measured 2026-11-20)

- **Docker Scout:** 0 fixable critical/high vulnerabilities; runs as `otelcol` (non-root); SBOM + provenance attestations present (SPDX SBOM + SLSA v1) [`docs/provenance/SCOUT_VERDICT.json`].
- **OSV cross-check:** no fixable critical/high vulnerability beyond Docker Scout's findings.
- **Signature:** cosign signature verified by ReadyStack's release gate before publishing; check it yourself with `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/otelcol:v0.165.0-CE-ubuntu24.04-r3` (signatures are recorded in the public Sigstore transparency log).
- **ReadyStack release check at release:** passed (ReadyStack's own test, not a Docker Scout grade). Vulnerabilities are disclosed every day and the image never changes, so it is re-checked daily, beside Docker Scout's own dated grade, at https://readystack.dev/queue.
- **Before exposing this service:** The OTLP receiver on 4317/4318 has no authentication in this distribution: publish it on localhost or a private network, or require client certificates (tls client_ca_file) in your own configuration.

## Reproducibility

Rebuilds offline from this archive to the identical image digest `sha256:c1d25f30f0f551220ae488008fdc7284f16a56dc427211d6c11bf7664890f48a`: run `./BUILD_OFFLINE.sh`, then `./VERIFY_OFFLINE.sh` (add `--online` to confirm the Docker Hub tag holds the same digest). Before publishing, ReadyStack rebuilt the kit with `./BUILD_OFFLINE.sh` and compared the result with the published digest [`docs/provenance/REPRO_VERIFICATION.json`], then ran `./BUILD_OFFLINE.sh` and `./VERIFY_OFFLINE.sh` from this archive (without `--online`).

## Validation

- 1 of 1 use cases passed (3 authored, 1 kept in the suite; 2 not exercised, see below) against the running image [`docs/validation/test-results.json`].
- The only case is `GET /` (200); the suite checks nothing else.
- 2 further authored checks were not exercised and are not counted above: 2 targeted ports 8888, 55679, which the build's test container does not publish, so they never reached the product (listed under Not exercised in `docs/validation/FAILED_TESTS.md`).

## Known issues

- **high** (upstream): Unauthenticated access by default [`RSD-otelcol-8b408d93b3`]

## Defects fixed

- 17 problems found before release and fixed before it shipped (17 by review); each is listed in `docs/defects/defects.json`.

## Verify this archive

```
sha256sum -c CHECKSUMS.sha256
./BUILD_OFFLINE.sh && ./VERIFY_OFFLINE.sh --online
cosign verify --key https://readystack.dev/keys/cosign.pub readystack/otelcol:v0.165.0-CE-ubuntu24.04-r3
```

ReadyStack records every signature in the public Sigstore transparency log (Rekor), so the command needs no extra flags.

