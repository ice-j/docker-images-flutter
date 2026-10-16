# Docker Images for [Flutter](https://flutter.dev/)

[![Flutter images][build_badge]][build_link]

Multi-arch (`linux/amd64`, `linux/arm64`) Docker images with the Flutter SDK and Android SDK preinstalled, published to `ghcr.io/ice-j/flutter`.

Images are built on top of [`ghcr.io/ice-j/android-sdk`](https://github.com/ice-j/docker-images-android/pkgs/container/android-sdk) and rebuilt automatically when a new Flutter stable or beta release is published.

## Tags

| Tag | Description |
| --- | --- |
| `stable`, `latest` | Latest Flutter stable release |
| `beta` | Latest Flutter beta release |
| `<version>` | A specific Flutter version, e.g. `3.47.7` or `3.50.0-0.1.pre` (`+` is replaced with `-`) |

The versions currently behind `stable` and `beta` are recorded in [`versions.json`](versions.json).

## Usage

Run locally by mounting the current directory:

```bash
docker run --rm -it -v ${PWD}:/build --workdir /build ghcr.io/ice-j/flutter:stable flutter test
```

Or use it as a job container in GitHub Actions:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    container: ghcr.io/ice-j/flutter:stable
    steps:
      - uses: actions/checkout@v4
      - run: flutter pub get
      - run: flutter test
```

## What's inside

- Flutter SDK at `/opt/flutter` (`FLUTTER_HOME`, `FLUTTER_ROOT`), with `flutter` and `dart` on `PATH`
- Android SDK at `/opt/android-sdk-linux` (`ANDROID_HOME`) with platform and build-tools 37, licenses accepted
- Android artifacts precached (`flutter precache --android`)
- JDK 21
- Flutter and Dart analytics disabled

## How updates work

The [`flutter-images`](.github/workflows/flutter-images.yml) workflow runs every 2 hours. It resolves the current stable and beta versions from Flutter's release feed, commits any change to `versions.json`, and builds and pushes images for versions that aren't published yet. Changes to `sdk/` trigger a full rebuild; a rebuild can also be forced from the Actions tab.

## Credits

Originally based on [cirruslabs/docker-images-flutter](https://github.com/cirruslabs/docker-images-flutter).

[build_badge]: https://github.com/ice-j/docker-images-flutter/actions/workflows/flutter-images.yml/badge.svg
[build_link]: https://github.com/ice-j/docker-images-flutter/actions/workflows/flutter-images.yml
