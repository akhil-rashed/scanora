# Scanora

<p align="center">
  <img src="https://i.ibb.co.com/4n0LPmHB/New-Project-DF44-C5-B.png" alt="Scanora" width="96">
</p>

<h3 align="center">A clean, private QR & barcode scanner built to stay out of the way.</h3>

<p align="center">
  Fast scanning · Local history · Gallery scanning · Smart results · No backend
</p>

<p align="center">
  <img src="https://img.shields.io/badge/platform-Android-111827?style=flat-square" alt="Android">
  <img src="https://img.shields.io/badge/version-1.0.0-2563EB?style=flat-square" alt="Version">
  <img src="https://img.shields.io/badge/Capacitor-7-111827?style=flat-square" alt="Capacitor 7">
  <img src="https://img.shields.io/badge/license-not%20specified-64748B?style=flat-square" alt="License">
</p>

---

## What is Scanora?

**Scanora** is a lightweight QR and barcode scanner designed around one simple idea:

> **Point. Scan. Done.**

It opens directly into the scanner instead of making the user walk through an intro or dashboard. The camera continuously looks for codes, while the interface keeps controls close, readable, and intentionally quiet.

The app also includes a local scan history, gallery-based scanning, smart result actions, filtering, sorting, search, TXT export, torch control, zoom, and multi-code detection.

There is no server, account system, or remote database in the project. Scan history is persisted locally on the device.

---

## ✦ Highlights

- **Live camera scanning** with automatic QR/barcode detection
- **Gallery scanning** for codes already saved as images
- **Automatic format recognition** — no manual QR/barcode mode switch
- **Multi-code detection** when multiple codes are visible
- **Local scan history** stored on-device
- **Search, filter & sort** scan history
- **Swipe-to-delete** history entries
- **TXT history export**
- **Copy & share** scan results
- **Smart actions** for recognized content
- **Torch control** when the device supports it
- **Camera zoom** when supported
- **Tap-to-focus** interaction
- **Duplicate-scan cooldown** to prevent noisy repeated entries
- **Native Android bridge support**
- **Capacitor Android support**
- **Offline 1D barcode fallback decoder**
- **Light/dark appearance** based on the device color scheme
- **Responsive, touch-first UI**
- **No images stored for scan history**

---

## Scanning

Scanora uses the browser/WebView `BarcodeDetector` API when it is available.

The application is configured to recognize:

| Format | Support |
|---|---|
| QR Code | ✓ |
| EAN-13 | ✓ |
| EAN-8 | ✓ |
| UPC-A | ✓ |
| UPC-E | ✓ |
| Code 128 | ✓ |
| Code 39 | ✓ |
| Code 93 | ✓ |
| ITF | ✓ |
| Codabar | ✓ |
| Data Matrix | ✓ |
| PDF417 | ✓ |
| Aztec | ✓ |

### Fallback decoding

When the native `BarcodeDetector` API is unavailable, Scanora includes a local JavaScript fallback decoder for common 1D formats.

The bundled fallback specifically implements:

- EAN-13
- UPC-A
- EAN-8 / other configured formats through the native detector when available
- Code 128 / Code 39 through the native detector when available

The fallback does **not** provide feature-parity with the native detector, so supported formats can vary by browser/WebView/device.

---

## Smart results

Scanora doesn't just dump the raw scanned string onto the screen.

It attempts to understand what the code contains and presents useful actions when possible.

### Recognized content types

- **URLs** — open the detected web address
- **Phone numbers** — call
- **Email addresses** — compose an email
- **SMS payloads** — open the messaging action
- **Wi-Fi payloads** — display network information
- **vCards** — display contact information
- **Geo coordinates** — open map/location actions
- **Plain text** — copy/share as text

The result screen also keeps the original scanned value available, so the app remains useful even when the payload is not one of the recognized structured types.

---

## History, without the cloud

Scanora keeps scan history locally.

The storage layer uses:

1. **IndexedDB** as the primary local database
2. **localStorage** as a fallback for restricted WebView environments

Stored scan records contain the scanned content, format, source, timestamp, and a generated local ID.

**Images are not stored as part of scan history.**

History can be:

- searched
- filtered by All / QR / Barcode
- sorted by newest or oldest
- grouped by QR or barcode
- opened for detailed results
- individually deleted
- cleared completely
- exported as a readable `.txt` file

---

## Privacy

Scanora is designed around local-first use.

There is no backend or account system in this repository, and the history database is local to the device.

> **Your scans stay on your device.**

Some result actions naturally leave the app — for example, opening a web URL, starting a phone call, composing an email, or opening a map. Those actions are handled by the operating system or the relevant external application.

---

## Gallery scanning

Don't have the physical code in front of you?

Scanora can scan a code from an image selected from the gallery.

The same detection pipeline is used to inspect the image, and the resulting scan can be saved to local history just like a camera scan.

---

## Android architecture

Scanora is built as a web-first application and packaged for Android with **Capacitor**.

```text
┌─────────────────────────────┐
│        Scanora UI           │
│       HTML / CSS / JS       │
└──────────────┬──────────────┘
               │
        NativeBridge
               │
      ┌────────┼─────────┐
      │        │         │
    Camera   Torch     Gallery
      │        │         │
      └────────┼─────────┘
               │
        Android / WebView
```

The `NativeBridge` abstraction keeps hardware-specific functionality separated from the main scanner logic.

It can work with:

- standard browser camera APIs
- Android bridge methods when available
- Capacitor / Android WebView environments

The same web code can therefore be reused across different deployment targets.

---

## Tech stack

Scanora intentionally keeps the stack small.

### Frontend

- HTML5
- CSS3
- Vanilla JavaScript
- SVG icons
- Web APIs

### Android

- Capacitor 7
- Android WebView
- Java/Kotlin-generated Android project through Capacitor

### Local storage

- IndexedDB
- localStorage fallback

### Automation

- GitHub Actions
- Node.js 20
- Java 21
- Capacitor Android build pipeline

No React.  
No Vue.  
No Tailwind.  
No Bootstrap.  
No backend.

Just a focused client-side application.

---

## Project structure

```text
scanora/
├── .github/
│   └── workflows/
│       └── build.yml
│
├── www/
│   ├── index.html
│   └── Ash.txt
│
├── index.html
├── package.json
├── capacitor.config.json
└── README.md
```

### `www/index.html`

The main application.

It contains the scanner UI, styles, detection logic, local history layer, result parsing, native bridge abstraction, gallery handling, export logic, and interaction controllers.

### `capacitor.config.json`

Defines the Android application identity and tells Capacitor that `www/` is the web application directory.

Current app identity:

```text
App name: Scanora
Package ID: com.scanora.app
Web directory: www
```

### `.github/workflows/build.yml`

Automates the Android build whenever changes are pushed to `main`, while also supporting manual workflow dispatch.

The workflow:

1. checks out the repository
2. installs Node.js
3. installs Java
4. installs npm dependencies
5. adds the Android platform
6. prepares/generates Android icons
7. adds camera permission
8. syncs the web application into Android
9. builds a release APK
10. signs the APK
11. uploads the APK as a workflow artifact
12. publishes a GitHub Release

---

## Run locally

### Requirements

- Node.js 20+
- npm
- Android Studio / Android SDK if building Android locally
- Capacitor CLI dependencies from `package.json`

### Install

```bash
npm install
```

### Add Android

```bash
npx cap add android
```

### Sync the web app

```bash
npx cap sync android
```

### Open Android Studio

```bash
npx cap open android
```

### Build from Android Studio

Use Android Studio's normal build/run tools after syncing the project.

---

## Build an APK with GitHub Actions

The repository already includes:

```text
.github/workflows/build.yml
```

Push the project to the `main` branch or manually run the workflow from the **Actions** tab.

The workflow produces:

- a `release.apk` artifact
- a GitHub Release containing the APK

### Production signing note

The current workflow creates its signing keystore inside CI. That is convenient for development/testing, but it is **not a good long-term production signing strategy** because Android app updates should use a persistent signing key.

For a production release, store the keystore and signing credentials securely in GitHub Secrets and reuse the same signing identity for future versions.

---

## Design philosophy

Scanora's interface is intentionally **modern without trying to look futuristic**.

The visual language focuses on:

- soft glass surfaces
- restrained blur
- clean typography
- subtle depth
- rounded controls
- calm blue accents
- responsive micro-interactions
- camera-first composition
- generous spacing
- clear hierarchy

The scanner screen uses a dark camera stage with a sculpted viewfinder, a subtle scanning sweep, compact status controls, and a floating bottom dock.

Secondary screens use a lighter ambient surface with soft blurred orbs and translucent cards.

The goal is not to make the app look like a spaceship.

It should simply feel **finished**.

---

## Interaction details

Small interactions are part of the experience rather than decoration.

Examples include:

- viewfinder compression when a code is detected
- detection color transition
- scanning sweep animation
- tap focus pulse
- spring-based button feedback
- torch state feedback
- zoom state updates
- swipe-to-delete history cards
- result sheets and confirmation modals
- toast feedback
- responsive bottom dock controls

The UI is designed around touch input first, with safe-area support for modern mobile devices.

---

## Configuration

Application-wide values are centralized in `APP_CONFIG` inside `www/index.html`.

```js
const APP_CONFIG = Object.freeze({
  APP_NAME: 'Scanora',
  APP_VERSION: '1.0.0',
  PRIVACY_STATEMENT: 'Your scans stay on your device.',
  DUPLICATE_COOLDOWN_MS: 2500,
  SCAN_INTERVAL_MS: 140,
  SUPPORTED_FORMATS: [...]
});
```

This makes common application settings easier to change without hunting through the entire codebase.

---

## Why Scanora?

A scanner doesn't need a complicated product tour, a giant dashboard, or an account before it lets you scan something.

Scanora keeps the flow simple:

```text
Open
  ↓
Point
  ↓
Detect
  ↓
Understand
  ↓
Act / Save
```

Everything else stays secondary.

---

## Roadmap

Possible future improvements include:

- richer barcode fallback coverage
- more structured payload types
- better accessibility controls
- customizable scan behavior
- configurable history retention
- persistent production signing
- broader platform packaging
- automated tests for decoding and payload parsing

---

## Contributing

Contributions, bug reports, and improvements are welcome.

If you're changing scanner behavior, please keep the following principles in mind:

- preserve the camera-first flow
- avoid unnecessary dependencies
- keep scan history local by default
- avoid storing camera frames
- keep interactions fast on mobile hardware
- maintain accessibility and touch-friendly targets
- prefer small, understandable changes over unnecessary abstraction

---

## License

No license is currently specified in the repository.

If this project is intended to be shared, forked, or redistributed publicly, add an appropriate `LICENSE` file before publishing under explicit open-source terms.

---

<p align="center">
  <strong>Scanora</strong><br>
  <sub>Scan less. Get more done.</sub>
</p>
