FROM ghcr.io/ice-j/android-sdk:37

LABEL org.opencontainers.image.source=https://github.com/ice-j/docker-images-flutter
USER root

ARG flutter_version

ENV FLUTTER_HOME=/opt/flutter \
    FLUTTER_VERSION=$flutter_version
ENV FLUTTER_ROOT=$FLUTTER_HOME

ENV PATH=${PATH}:${FLUTTER_HOME}/bin:${FLUTTER_HOME}/bin/cache/dart-sdk/bin

RUN test -n "${FLUTTER_VERSION}" || { echo "flutter_version build arg is required" >&2; exit 1; } \
    && git config --system --add safe.directory '*' \
    && git clone --depth 1 --branch ${FLUTTER_VERSION} https://github.com/flutter/flutter.git ${FLUTTER_HOME}

RUN flutter config --no-analytics \
    && dart --disable-analytics \
    && yes | flutter doctor --android-licenses \
    && flutter doctor

RUN flutter precache --android
