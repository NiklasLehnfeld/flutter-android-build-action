# flutter-android-build-action
🏗 Easy to use flutter android build action

You can find a complete usage example in the [netzpolitik.org app repository](https://github.com/NiklasLehnfeld/netzpolitik-mobile/blob/main/.github/workflows/main.yaml)

## Requirements
This action runs 
```
flutter pub run build_runner build --delete-conflicting-outputs
```
before it starts. Means you need the [`build_runner`](https://pub.dev/packages/build_runner) package to be installed.

## Usage

```yaml
on:
  push:
    branches:
      - main
name: Test
jobs:
  android-release-build:
    name: android-release-build
    runs-on: ubuntu-latest
    container: ghcr.io/niklaslehnfeld/flutter-essentials-docker:master
    steps:
      - uses: actions/checkout@v2
      - uses: NiklasLehnfeld/flutter-android-build-action@v1
        id: android-build
        with:
          release: true
          keystore: ${{ secrets.KEYSTORE }}
          keystore-pwd: "${{ secrets.KEYSTORE_PASSWORD }}"
          key-pwd: "${{ secrets.KEY_PASSWORD }}"
      - name: Archive APK
        uses: actions/upload-artifact@v2
        with:
          name: release-apk
          path: ${{ steps.android-build.outputs.apk-path }}
      - name: Archive AAB
        uses: actions/upload-artifact@v2
        with:
          name: release-aab
          path: ${{ steps.android-build.outputs.aab-path }}
```

## Parameters
```yaml
input:
  release:
    description: 'If set to `true`, it will run release builds, otherwise debug builds'
    required: false
    default: false
  build-type:
    description: 'apk | aab | both'
    required: false
    default: both
  keystore:
    description: 'Base64 encoded keystore.jks, required if release is true'
    required: false
    default: ''
  keystore-pwd:
    description: 'keystore password, required if release is true'
    required: false
    default: ''
  key-pwd:
    description: 'key password, required if release is true'
    required: false
    default: ''
```

## Outputs
```yaml
outputs:
  apk-path:
    description: "Path to built apk if build-type was set to apk or both"
    value: ${{ steps.path-output.outputs.apk-path }}
  aab-path:
    description: "Path to built aab if build-type was set to aab or both"
    value: ${{ steps.path-output.outputs.aab-path }}
```
