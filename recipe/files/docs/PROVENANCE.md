# Provenance — OpenTelemetry Collector (`v0.165.0-CE-ubuntu24.04-r3`)

Every item below is copied from a record in this archive; the record is named in brackets.

## Source

- Upstream repository: https://github.com/open-telemetry/opentelemetry-collector-releases
- Revision built: `v0.165.0` (commit `b9425f263115f328bc3bd4e65b825101b499f144`)
- Source tree: `b557a9d720177cf7dddb0127a892960b854e2363a6898f4213f1e2f61f20e0b8` over 181 files [`docs/provenance/SOURCE_TREE_HASH.json`]
- Community Edition only: enterprise code is removed or refused before the build (CE audit at acquire).

## Build

- Method: `source-rebuild-go`; base image `scratch + a chiselled Ubuntu 24.04 runtime (vendored/chisel/slices.txt, packages.tsv)`
- Compiled with `--network=none` from vendored source, dependencies and base images; BuildKit pinned to `moby/buildkit:v0.31.2` (shipped in `toolchain/`).
- SOURCE_DATE_EPOCH: `1795161176` [`SOURCE_DATE_EPOCH.txt`]
- Frozen inputs: [`docs/provenance/FREEZE_MANIFEST.json`], build recipe: [`docs/provenance/BUILD_RECIPE.json`].

## Output

- Published image: `readystack/otelcol:v0.165.0-CE-ubuntu24.04-r3` — index digest `sha256:ba73b28251588975942fdc07d13ed79c046c4e43a30d4244a8087458c5a1604f` [`docs/provenance/LOCKED_DIGEST.txt`]
- linux/amd64 image digest `sha256:c1d25f30f0f551220ae488008fdc7284f16a56dc427211d6c11bf7664890f48a` — reproduced from this archive before publishing [`docs/provenance/REPRO_VERIFICATION.json`].

## Attestations and signature

- SBOM + SLSA provenance attached to the image: attached: an SPDX SBOM and SLSA v1 provenance [`docs/provenance/SCOUT_VERDICT.json`]
- cosign signature by the ReadyStack release key: verified before publishing. Check it: `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/otelcol:v0.165.0-CE-ubuntu24.04-r3`

## Pinned inputs, and why

- 26 pinned inputs, each with where it came from, why this one, how it was checked and when it was decided: `docs/PINS.md` [`docs/provenance/PINS.json`] (upstream source 2, base and build images 3, image digests 2, dependency versions readystack sets 5, pins in the kit's dockerfiles 7, readystack's build engine 5, dependencies upstream chose 2).

## Build trail

- How this release passed through the ReadyStack harness, record by record (every chain, drive, stage, review round, defect and check, each naming the record it is copied from): `ReadyStack-otelcol-v0.165.0-CE-ubuntu24.04-r3.trail.json`, published beside this archive and signed with the release key (`ReadyStack-otelcol-v0.165.0-CE-ubuntu24.04-r3.trail.json.sigstore`), and readable at https://readystack.dev/products/otelcol/trail/v0.165.0-CE-ubuntu24.04-r3.

