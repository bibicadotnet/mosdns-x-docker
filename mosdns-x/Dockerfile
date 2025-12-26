FROM alpine:latest

ARG TARGETARCH

RUN apk add --no-cache curl unzip ca-certificates tzdata

# Detect mosdns-x arch
RUN case "${TARGETARCH}" in \
    amd64)  MOSDNS_ARCH="amd64" ;; \
    arm64)  MOSDNS_ARCH="arm64" ;; \
    arm/v7) MOSDNS_ARCH="armv7" ;; \
    *) echo "Unsupported arch: ${TARGETARCH}" && exit 1 ;; \
    esac && \
    echo "Using arch: ${MOSDNS_ARCH}" && \
    LATEST=$(curl -s https://api.github.com/repos/pmkol/mosdns-x/releases/latest \
      | sed -n 's/.*"tag_name":[[:space:]]*"\([^"]*\)".*/\1/p') && \
    echo "Installing mosdns-x ${LATEST}" && \
    curl -sL "https://github.com/pmkol/mosdns-x/releases/download/${LATEST}/mosdns-linux-${MOSDNS_ARCH}.zip" \
      -o /tmp/mosdns.zip && \
    unzip -qo /tmp/mosdns.zip -d /usr/local/bin && \
    chmod +x /usr/local/bin/mosdns && \
    rm -f /tmp/mosdns.zip

WORKDIR /home/mosdns-x

EXPOSE 53/tcp
EXPOSE 53/udp
EXPOSE 853/tcp
EXPOSE 853/udp
EXPOSE 8090/tcp
EXPOSE 8090/udp

CMD ["mosdns", "start", "-c", "/home/mosdns-x/config/config.yaml", "-d", "/home/mosdns-x"]
