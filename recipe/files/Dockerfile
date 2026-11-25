# GENERATED L-009 hermetic build (--network=none, SBOM+SLSA-attestation-producing).
# Pinned bases from BASE_IMAGE_DIGESTS.lock; vendored Go tree + debs from Dockerfile.vendor.
FROM golang:1.26.11-bookworm@sha256:dc40a13b61304647ed7e5d581d246b86413dfc29d63c6d018310feb93cd98fc0 AS build_otelcol
COPY vendored/otelcol-src/ /src/
WORKDIR /src/distributions/otelcol/_build
ENV CGO_ENABLED=0 GOTOOLCHAIN=local GOWORK=off GOFLAGS="-mod=vendor -buildvcs=false" GOPROXY=off SOURCE_DATE_EPOCH=1795161176
RUN go build -trimpath -tags "grpcnotrace" -ldflags='-s -w' -o /otelcol . && /otelcol --version > /tmp/otelcol.version 2>&1; cat /tmp/otelcol.version; grep -qF 'otelcol version 0.165.0' /tmp/otelcol.version || { echo 'otelcol --version does not report otelcol version 0.165.0'; exit 1; } && \
    { readelf -lW /otelcol | sed -n 's/.*Requesting program interpreter: \(.*\)]$/\1/p'; readelf -dW /otelcol | sed -n 's/.*Shared library: \[\(.*\)\]$/\1/p'; } > /otelcol.needs
# rs-probe: the HEALTHCHECK helper (standard library only, from the kit's rs-probe.go)
FROM golang:1.26.11-bookworm@sha256:dc40a13b61304647ed7e5d581d246b86413dfc29d63c6d018310feb93cd98fc0 AS build_rs_probe
WORKDIR /probe
COPY rs-probe.go main.go
COPY rs-probe.mod go.mod
ENV CGO_ENABLED=0 GOTOOLCHAIN=local GOFLAGS="-buildvcs=false" GOPROXY=off SOURCE_DATE_EPOCH=1795161176
RUN go build -trimpath -ldflags='-s -w' -o /rs-probe .
# the chiselled rootfs (vendored/chisel, cut by Dockerfile.vendor), the service user, the binaries
FROM ubuntu:24.04@sha256:9c7f200c07e3d4a03e7249d2c16bf66a51120fb876aa2242cf31451412543289 AS rootfs
ARG SOURCE_DATE_EPOCH=1795161176
ADD vendored/chisel/rootfs.tar /rootfs/
RUN printf 'otelcol:x:999:999::/var/lib/otelcol:/bin/sh\n' >> /rootfs/etc/passwd && printf 'otelcol:x:999:\n' >> /rootfs/etc/group && \
    mkdir -p /rootfs/var/lib/otelcol /rootfs/etc/otelcol && chown -R 999:999 /rootfs/var/lib/otelcol /rootfs/etc/otelcol
COPY --from=build_otelcol /otelcol /rootfs/usr/bin/otelcol
COPY --from=build_otelcol /otelcol.needs /tmp/needs/otelcol
COPY --from=build_rs_probe /rs-probe /rootfs/usr/local/bin/rs-probe
COPY config.yaml /rootfs/etc/otelcol/config.yaml
RUN chmod 755 /rootfs/usr/bin/otelcol /rootfs/usr/local/bin/rs-probe && \
    chown 999:999 /rootfs/etc/otelcol/config.yaml && \
    chmod 0644 /rootfs/etc/otelcol/config.yaml
RUN --mount=type=bind,source=vendored/chisel,target=/chisel set -eu; \
    for n in /tmp/needs/*; do while read -r lib; do [ -n "$lib" ] || continue; \
      case "$lib" in /*) [ -e "/rootfs$lib" ] ;; *) [ -n "$(find /rootfs/usr/lib /rootfs/usr/lib64 -name "$lib" 2>/dev/null | head -1)" ] ;; esac || \
      { echo "$(basename "$n") needs $lib, which the chiselled rootfs does not have: add its slice to runtime_base.slices" >&2; exit 1; }; \
    done < "$n"; done; rm -rf /tmp/needs; \
    [ -e /rootfs/etc/os-release ] || { echo "no /etc/os-release: base-files_release-info is missing" >&2; exit 1; }; \
    [ "$(grep -c '^Package: ' /rootfs/var/lib/dpkg/status)" = "$(wc -l < /chisel/packages.tsv)" ] || \
      { echo "/var/lib/dpkg/status does not list every package of vendored/chisel/packages.tsv" >&2; exit 1; }; \
    rm -rf /rootfs/var/log/dpkg.log /rootfs/var/log/alternatives.log /rootfs/var/log/apt /rootfs/var/cache/ldconfig/aux-cache /rootfs/var/cache/fontconfig
FROM scratch
ARG SOURCE_DATE_EPOCH=1795161176
COPY --from=rootfs /rootfs/ /
ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
LABEL org.opencontainers.image.licenses="Apache-2.0"
LABEL org.opencontainers.image.source="https://github.com/open-telemetry/opentelemetry-collector-releases"
LABEL org.opencontainers.image.title="OpenTelemetry Collector"
LABEL org.opencontainers.image.description="The OpenTelemetry Collector receives traces, metrics and logs from your applications over OTLP, processes them, and forwards them to the observability backends you choose. This is the vendor-neutral core distribution, shipped with its diagnostic endpoints closed to the network."
LABEL org.opencontainers.image.version="v0.165.0-CE-ubuntu24.04-r3"
LABEL org.opencontainers.image.revision="b9425f263115f328bc3bd4e65b825101b499f144"
HEALTHCHECK --interval=5s --timeout=30s --start-period=10s \
  CMD /usr/local/bin/rs-probe http://127.0.0.1:13133/ || exit 1
USER otelcol
EXPOSE 4317 4318 13133
ENTRYPOINT ["otelcol"]
CMD ["--config", "/etc/otelcol/config.yaml"]
