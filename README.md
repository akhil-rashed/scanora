<div align="center">

<img src="[https://i.ibb.co.com/zh9zp3fq/file-00000000bfcc8211aa51fb9341c6096d.png](https://i.ibb.co.com/Psbmh1M0/file-00000000c1ac82119c142444655f4aca.png)" alt="Scanora logo" width="96" height="96" />

# Scanora

**Scan any QR code or barcode. Instantly. Privately.**

A fast, friendly scanner that lives on your phone, works offline, and keeps every scan on your device.

<br />

[![Download APK](https://img.shields.io/badge/Download-Latest_APK-2563eb?style=for-the-badge&logo=android&logoColor=white)](../../releases/latest)

<br />

![Android](https://img.shields.io/badge/Android-Capacitor_7-3ddc84?style=flat-square&logo=android&logoColor=white)
![JavaScript](https://img.shields.io/badge/Vanilla-JavaScript-f7df1e?style=flat-square&logo=javascript&logoColor=black)
![Offline](https://img.shields.io/badge/Works-Offline-2563eb?style=flat-square)
![No tracking](https://img.shields.io/badge/Tracking-None-16a34a?style=flat-square)
![Build](https://img.shields.io/github/actions/workflow/status/akhil-rashed/scanora/build.yml?style=flat-square&label=build)

</div>

<br />

## Why Scanora?

Most scanner apps are packed with ads, pop-ups and permissions they have no business asking for. Scanora does one thing and does it well: point your camera, get the result, act on it.

There is no account, no cloud and no analytics. Your scan history stays on your phone, and nowhere else.

<br />

## Highlights

**Scanning**
- Live camera scanning with a clean, distraction-free viewfinder
- Pinch to zoom, tap to focus and a one-tap flashlight
- Scan from a photo in your gallery
- Pick the right code when several appear in the same frame
- Smart duplicate protection, so one code never fires twice in a row

**Understands what it scans**

| You scan | Scanora offers |
| --- | --- |
| Website link | Open link, Copy |
| Wi-Fi network | Copy password, Copy network name |
| Contact card (vCard) | Save contact, Copy phone |
| SMS | Send message, Copy |
| Location | Open in Maps, Copy coordinates |
| Email | Send email, Copy |
| Phone number | Call, Copy |
| Plain text | Copy, Share |

**History that stays out of your way**
- Every scan is saved automatically on your device
- Search by content, format or date
- Filter by QR or Barcode, and sort by newest, oldest or type
- Swipe a card to delete it
- Export your history as a `.txt` file
- Clear everything in one tap

**Little touches**
- Soft scan chime, easy to mute
- Light and dark theme that follows your phone
- Frosted-glass interface with smooth, springy motion
- Friendly, clear messages when camera access is denied or unavailable

<br />

## Supported formats

| 2D codes | 1D barcodes |
| --- | --- |
| QR Code | EAN-13, EAN-8 |
| Data Matrix | UPC-A, UPC-E |
| PDF417 | Code 128, Code 39, Code 93 |
| Aztec | ITF, Codabar |

Detection uses the native `BarcodeDetector` API where the device supports it. A lightweight built-in decoder takes over for common retail barcodes when it doesn't, with no network involved.

<br />

## Privacy first

- Scans are stored in on-device IndexedDB, with a localStorage fallback
- Nothing is uploaded, synced or shared unless you press Share yourself
- Camera access is requested only to scan, and the app explains why before asking
- Links are validated before they are opened

<br />

## Get the app

### Install on Android

1. Open the [**Releases**](../../releases) page
2. Download the latest `release.apk`
3. Open it on your phone and allow installation from your browser if prompted

Every push to `main` builds and publishes a fresh release automatically.

### Try it in a browser

Scanora is a single static page, so any local server will do. The camera works on `localhost`.

```bash
git clone https://github.com/akhil-rashed/scanora.git
cd scanora
npx serve www
```

<br />

## Build it yourself

**Requirements:** Node.js 20+, JDK 21, Android Studio (or the Android SDK)

```bash
# 1. Install dependencies
npm install

# 2. Add the Android platform
npx cap add android

# 3. Copy the web app into the native project
npx cap sync android

# 4. Open in Android Studio and run
npx cap open android
```

Add the camera permission to `android/app/src/main/AndroidManifest.xml` if it isn't there already:

```xml
<uses-permission android:name="android.permission.CAMERA" />
<uses-feature android:name="android.hardware.camera" android:required="false" />
```

<br />

## Project structure

```
scanora/
├── www/                      Web app that Capacitor ships
│   └── index.html            UI, styles and all app logic in one file
├── index.html                Source copy of the app
├── capacitor.config.json     App id, name and web directory
├── package.json              Capacitor dependencies
└── .github/workflows/
    └── build.yml             CI that builds, signs and releases the APK
```

<br />

## How it works

```
Camera / Gallery
      │
      ▼
 ScannerEngine ──► BarcodeDetector (native)
      │                  │ not available?
      │                  ▼
      │            OfflineDecoder (pure JS)
      ▼
 Classifier ──► URL · Wi-Fi · vCard · SMS · Geo · Email · Phone · Text
      │
      ▼
 Result sheet ──► Actions ──► History (IndexedDB)
```

A small `NativeBridge` layer keeps camera, torch, gallery and file export separate from the UI, so the same code runs as a web page, a Capacitor app, or inside an Android WebView.

<br />

## Tech stack

- **App:** HTML, CSS and vanilla JavaScript, with no framework and no runtime dependencies
- **Native shell:** [Capacitor](https://capacitorjs.com) 7
- **Storage:** IndexedDB
- **CI/CD:** GitHub Actions, with automatic APK signing and GitHub Releases

<br />

## Roadmap

- [ ] QR code generator
- [ ] Batch scanning mode
- [ ] Export history as CSV
- [ ] Installable web version (PWA)
- [ ] More languages

<br />

## Contributing

Ideas, bug reports and pull requests are all welcome.

1. Fork the repository
2. Create a branch: `git checkout -b feature/your-idea`
3. Commit your changes and push the branch
4. Open a pull request

<br />

<div align="center">

Made by [Akhil Rashed](https://github.com/akhil-rashed)

If Scanora is useful to you, a star on the repo means a lot.

</div>
