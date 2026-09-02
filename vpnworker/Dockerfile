FROM python:3.11-slim-bullseye

ENV TZ=Asia/Taipei
ENV HOME=/vpn-home
ENV USER=root
ENV LOGNAME=root
ENV SHELL=/bin/sh

ENV CHROME_BIN=/usr/bin/chromium
ENV CHROMEDRIVER_BIN=/usr/bin/chromedriver

WORKDIR /app

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        curl \
        ca-certificates \
        chromium \
        chromium-driver \
        iproute2 \
        iptables \
        procps \
        bsdextrautils && \
    rm -rf /var/lib/apt/lists/*

RUN curl -fsSL \
    https://raw.githubusercontent.com/AdguardTeam/AdGuardVPNCLI/HEAD/scripts/release/install.sh \
    | sh -s -- -v -a y && \
    ADGUARD_BIN="$(find /opt /usr/bin /usr/local/bin -type f -name adguardvpn-cli 2>/dev/null | head -n 1)" && \
    if [ -n "$ADGUARD_BIN" ]; then \
        ln -sf "$ADGUARD_BIN" /usr/local/bin/adguardvpn-cli; \
    fi && \
    command -v adguardvpn-cli

RUN pip install --no-cache-dir \
    selenium \
    flask \
    requests \
    beautifulsoup4

CMD ["python3", "-u", "update_worker.py"]