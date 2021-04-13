# GitHub Actions for MobSF

This Action for [MobSF](https://github.com/MobSF/Mobile-Security-Framework-MobSF) enables MobSF analysis actions using the MobSF docker.

## Inputs

* `INPUT_FILE_NAME` - **Required** - The input fila path to be analysed
* `SCAN_TYPE` - **Required** - the scan type: apk, zip, ipa, or appx
* `OUTPUT_FILE_NAME` - **Required** - the output file path (will output two files, the first with extension .json and the second .pdf)

## Example

To build a Flutter project with MobSF Analysis:

```yaml
name: Build and Deploy
on:
  push:
    branches:
      - master

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repo
        uses: actions/checkout@master

      - name: Set-up Java
        uses: actions/setup-java@v1
        with:
          java-version: '12.x'

      - name: Set-up Flutter
        uses: subosito/flutter-action@v1
        with:
          flutter-version: '1.9.1+hotfix.6'

      - name: Flutter Install Dependencies
      - run: flutter pub get

      - name: Flutter Test
        run: flutter test

      - name: Flutter Build
        run: flutter build apk

      - name: Run MobSF Analysis
        uses: fundacaocerti/mobsf-action@v1.6
        env:
          INPUT_FILE_NAME: build/app/outputs/apk/app.apk
          SCAN_TYPE: apk
          OUTPUT_FILE_NAME: mobsf-report

      - name: Upload MobSF Analysis PDF Result
        uses: actions/upload-artifact@v2
        with:
          name: mobsf-report.pdf
          path: mobsf-report.pdf

      - name: Upload MobSF Analysis JSON Result
        uses: actions/upload-artifact@v2
        with:
          name: mobsf-report.json
          path: mobsf-report.json
```
Alternatively:

```yaml
        with:
          SCAN_TYPE: ipa
```

Thanks @ajinabraham for the support on troubleshooting this development.

## License

The Dockerfile and associated scripts and documentation in this project are released under the [GPL-3.0](LICENSE).
