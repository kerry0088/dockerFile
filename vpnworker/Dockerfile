FROM python:3.11-slim-bullseye

ENV TZ=Asia/Taipei \
    HOME=/vpn-home \
    USER=root \
    LOGNAME=root \
    SHELL=/bin/sh \
    ADGUARD_LOCATION=Tokyo \
    FORMAL_DB_PATH=/data/av.db \
    CHROME_BIN=/usr/bin/chromium \
    CHROMEDRIVER_BIN=/usr/bin/chromedriver \
    DMM_CRAWLER_WORKERS=1

WORKDIR /app

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        curl \
        ca-certificates \
        chromium \
        chromium-driver \
        iproute2 \
        iptables \
        procps \
        bsdextrautils \
    && rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir \
    selenium \
    flask \
    requests \
    beautifulsoup4

CMD ["sh", "-c", "\
if ! command -v adguardvpn-cli >/dev/null 2>&1; then \
    echo 'Installing AdGuard VPN CLI in runtime environment...'; \
    curl -fsSL https://raw.githubusercontent.com/AdguardTeam/AdGuardVPNCLI/HEAD/scripts/release/install.sh | sh -s -- -v -a y; \
fi; \
if ! command -v adguardvpn-cli >/dev/null 2>&1; then \
    ADGUARD_BIN=$(find /opt /usr/bin /usr/local/bin -type f -name adguardvpn-cli 2>/dev/null | head -n 1); \
    if [ -n \"$ADGUARD_BIN\" ]; then \
        ln -sf \"$ADGUARD_BIN\" /usr/local/bin/adguardvpn-cli; \
    fi; \
fi; \
command -v adguardvpn-cli || { echo 'adguardvpn-cli not found after install'; exit 1; }; \
exec python3 -u update_worker.py \
"]
