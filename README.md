.github/workflows/android.yml
name: Gerar APK Android

on:
  workflow_dispatch:
  push:
    branches:
      - principal

permissions:
  contents: read

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Baixar projeto
        uses: actions/checkout@v4

      - name: Preparar Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Extrair projeto
        run: |
          unzip -q meu_planejamento_android_projeto.zip -d projeto

      - name: Preparar Gradle
        uses: gradle/actions/setup-gradle@v6
        with:
          gradle-version: '8.9'

      - name: Compilar APK
        run: |
          gradle -p projeto/meu_planejamento_android assembleDebug

      - name: Disponibilizar APK
        uses: actions/upload-artifact@v4
        with:
          name: Meu-Planejamento-APK
          path: projeto/meu_planejamento_android/app/build/outputs/apk/debug/*.apk
