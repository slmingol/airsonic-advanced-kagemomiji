FROM eclipse-temurin:17-jdk AS builder
WORKDIR target

COPY target/dependency/airsonic-main.war airsonic.war

FROM ghcr.io/linuxserver/baseimage-ubuntu:jammy

LABEL description="Airsonic-Advanced is a free, web-based media streamer, providing ubiquitous access to your music." \
      url="https://github.com/slmingol/airsonic-advanced"

ENV AIRSONIC_ADVANCED_HOME="/app/airsonic" \
    AIRSONIC_ADVANCED_SETTINGS="/config" \
    LANG="C.UTF-8"

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        ffmpeg \
        flac \
        fonts-dejavu \
        lame \
        netcat-openbsd \
        openjdk-17-jre-headless \
    && mkdir -p /app/airsonic \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

COPY --from=builder target/airsonic.war /app/airsonic/airsonic.war
COPY root/ /

EXPOSE 4040

VOLUME /config /music /playlists /podcasts

HEALTHCHECK --interval=15s --timeout=3s CMD curl -sf http://localhost:4040/rest/ping || false
