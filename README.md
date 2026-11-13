name: Build Cube Flip APK

on:
  workflow_dispatch:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Setup Android SDK
        uses: android-actions/setup-android@v3

      - name: Install SDK
        run: sdkmanager "platforms;android-35" "build-tools;35.0.0"

      - name: Setup Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.9'

      - name: Extract ZIP
        run: |
          mkdir project
          unzip -q CubeFlip_Fixed_Project.zip -d project

      - name: Build APK
        run: |
          cd project
          gradle assembleDebug

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: CubeFlip-APK
          path: project/app/build/outputs/apk/debug/app-debug.apk
          
