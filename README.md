# boost-spirituel
boost-spirituel/ ├── .github/ │   └── workflows/ │       └── build-apk.yml ├── www/ │   └── index.html ├── capacitor.config.json └── package.json
{
  "name": "boost-spirituel",
  "version": "1.0.0",
  "scripts": {
    "build": "echo 'No build step required'"
  },
  "dependencies": {
    "@capacitor/android": "^6.0.0",
    "@capacitor/cli": "^6.0.0",
    "@capacitor/core": "^6.0.0"
  }
}{
  "appId": "org.feudureveil.boostspirituel",
  "appName": "Boost Spirituel",
  "webDir": "www"
}github/workflows/build-apk.yml
name: Build Android APK

on:
  push:
    branches: [ main, master ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Setup Java JDK
        uses: actions/setup-java@v4
        with:
          distribution: 'zulu'
          java-version: '17'

      - name: Install Dependencies
        run: npm install

      - name: Add Android Platform
        run: npx cap add android

      - name: Sync Capacitor
        run: npx cap sync android

      - name: Build Debug APK
        run: |
          cd android
          chmod +x ./gradlew
          ./gradlew assembleDebug

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: Boost-Spirituel-APK
          path: android/app/build/outputs/apk/debug/app-debug.apk
