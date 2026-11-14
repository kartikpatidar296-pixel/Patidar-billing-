
name: PATIDAR BILLING Build

on:
  workflow_dispatch:
  push:
    branches: [main]

jobs:
  android:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Extract Flutter source
        run: |
          mkdir app
          unzip -o "$(find . -maxdepth 1 -name '*.zip' | head -n 1)" -d app

      - uses: subosito/flutter-action@v2
        with:
          channel: stable

      - name: Prepare Android project
        working-directory: app
        run: flutter create . --platforms=android

      - name: Build APK
        working-directory: app
        run: |
          flutter pub get
          flutter build apk --release

      - uses: actions/upload-artifact@v4
        with:
          name: PATIDAR-BILLING-APK
          path: app/build/app/outputs/flutter-apk/app-release.apk

  windows:
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v4

      - name: Extract Flutter source
        shell: pwsh
        run: |
          New-Item -ItemType Directory -Force app
          $zip = Get-ChildItem -Filter *.zip | Select-Object -First 1
          Expand-Archive -Path $zip.FullName -DestinationPath app -Force

      - uses: subosito/flutter-action@v2
        with:
          channel: stable

      - name: Prepare Windows project
        working-directory: app
        run: flutter create . --platforms=windows

      - name: Build Windows EXE
        working-directory: app
        run: |
          flutter pub get
          flutter build windows --release

      - uses: actions/upload-artifact@v4
        with:
          name: PATIDAR-BILLING-WINDOWS
          path: app/build/windows/x64/runner/Release/
# Patidar-billing-
Hallo
