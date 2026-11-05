name: build
on:
  push:
  workflow_dispatch:
jobs:
  apk:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: 17
      - uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: 8.9
      - run: |
          unzip -q ./*.zip -d x
          mkdir proj
          cp -r "$(dirname "$(find x -name settings.gradle.kts | head -1)")"/. proj/
      - run: gradle assembleDebug
        working-directory: proj
      - uses: actions/upload-artifact@v4
        with:
          name: MedAlarm-apk
          path: proj/app/build/outputs/apk/debug/app-debug.apk
