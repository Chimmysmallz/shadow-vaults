# Building the Shadow Vaults Android app

This folder wraps the web game in a native Android shell using
**[Capacitor](https://capacitorjs.com/)**, so you can produce a real installable
`.apk` (or `.aab` for the Play Store). The game itself is unchanged — Capacitor
just runs `index.html` inside a native WebView. Your saved progress
(`localStorage`) persists between app launches automatically.

> Prefer **no build tools at all**? You don't need this folder. Just host the
> parent folder over HTTPS and use Chrome on Android → **Install app**. The PWA
> installs to the home screen, runs offline, and saves progress. Use the steps
> below only if you specifically want an APK / Play Store build.

---

## Prerequisites

| Tool | Notes |
|---|---|
| **Node.js 18+** | <https://nodejs.org> |
| **Android Studio** | <https://developer.android.com/studio> (includes the Android SDK) |
| **JDK 17** | Bundled with recent Android Studio |

## Steps

From inside this `android/` folder:

```bash
# 1. Install Capacitor
npm install

# 2. Copy the game's web files into ./www
npm run sync

# 3. Generate the native Android project (first time only)
npx cap add android

# 4. Push the web assets + config into the native project
npx cap sync android

# 5. Open it in Android Studio
npx cap open android
```

In Android Studio:

- **Run** ▶ on a connected device or emulator to play immediately, **or**
- **Build → Build Bundle(s) / APK(s) → Build APK(s)** to get an installable file
  (find it under `android/app/build/outputs/apk/`).

### Re-building after you change the game
Edit the game in the parent folder, then:

```bash
npm run sync && npx cap sync android
```

## App icon & splash (optional but recommended)

Use the included `icon-512.png` to generate all Android densities:

```bash
npm install -g @capacitor/assets
# place a 1024x1024 icon at ./assets/icon.png first
npx @capacitor/assets generate --android
```

## Configuration

- **App name / ID:** edit `capacitor.config.json` (`appName`, `appId`).
  Current ID: `ng.shadowvaults.app` — change to your own reverse-domain ID
  before publishing.
- **Orientation:** the game is designed for **landscape**. To lock it, add this
  inside the `<activity>` tag of
  `android/app/src/main/AndroidManifest.xml` after `cap add android`:

  ```xml
  android:screenOrientation="landscape"
  ```

## Publishing to Google Play

1. In Android Studio: **Build → Generate Signed Bundle / APK → Android App Bundle (.aab)**.
2. Create a keystore when prompted and **keep it safe** — you need it for every update.
3. Upload the `.aab` at <https://play.google.com/console> (one-time $25 developer fee).

---

That's it — you now have Shadow Vaults as a native Android app. 🗝️
