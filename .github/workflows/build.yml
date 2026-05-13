name: Build RCIS App

on:
  push:
    branches: [ main, master ]
  workflow_dispatch:

jobs:

  # ── ANDROID APK ────────────────────────────────────────────────
  build-android:
    name: Build Android APK
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-java@v4
        with:
          distribution: 'zulu'
          java-version: '17'

      - uses: subosito/flutter-action@v2
        with:
          flutter-version: '3.22.2'
          channel: 'stable'
          cache: true

      - name: Get dependencies
        run: flutter pub get

      - name: Generate app icons (logo fix)
        run: dart run flutter_launcher_icons

      - name: Generate splash screen
        run: dart run flutter_native_splash:create

      - name: Add gradle properties
        run: |
          echo "android.useAndroidX=true" >> android/gradle.properties
          echo "android.enableJetifier=true" >> android/gradle.properties
          echo "org.gradle.jvmargs=-Xmx4g" >> android/gradle.properties

      # Build ONE universal FAT APK — works on ALL Android phones including
      # Unisoc T7250 (Itel City 100), MediaTek, Qualcomm, all architectures
      - name: Build Universal APK
        run: flutter build apk --release

      # Rename APK to start with RCIS
      - name: Rename APK
        run: |
          mkdir -p apk_output
          cp build/app/outputs/flutter-apk/app-release.apk apk_output/RCIS-v1.0-universal.apk

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: RCIS-Android-APK
          path: apk_output/RCIS-v1.0-universal.apk
          retention-days: 30

  # ── WINDOWS EXE + INSTALLER ────────────────────────────────────
  build-windows:
    name: Build Windows EXE + Installer
    runs-on: windows-latest

    steps:
      - uses: actions/checkout@v4

      - uses: subosito/flutter-action@v2
        with:
          flutter-version: '3.22.2'
          channel: 'stable'
          cache: true

      - name: Enable Windows desktop
        run: flutter config --enable-windows-desktop

      - name: Get dependencies
        run: flutter pub get

      - name: Generate app icons
        run: dart run flutter_launcher_icons

      - name: Build Windows EXE
        run: flutter build windows --release

      # Create portable ZIP (the app folder, not just EXE)
      - name: Create portable ZIP
        shell: pwsh
        run: |
          Compress-Archive -Path build\windows\x64\runner\Release\* `
            -DestinationPath RCIS-Windows-Portable.zip

      # Build proper installer using Inno Setup (pre-installed on GitHub runners)
      - name: Build Windows Installer
        shell: pwsh
        run: |
          $inno = "C:\Program Files (x86)\Inno Setup 6\ISCC.exe"
          if (Test-Path $inno) {
            & $inno windows\RCIS_installer.iss
            Write-Host "Installer built successfully"
          } else {
            Write-Host "Inno Setup not found, skipping installer"
          }

      - name: Upload Windows artifacts
        uses: actions/upload-artifact@v4
        with:
          name: RCIS-Windows
          path: |
            RCIS-Windows-Portable.zip
            installer_output/RCIS_Setup_v1.0.exe
          retention-days: 30
