# Shaheen Video Downloader — Android

The same UI and the same download engine as your desktop app, packaged as an
Android app (Android 7.0+, API 24 and newer; phones and tablets; ARM and x86).

## How it works

| Layer | What it is |
|---|---|
| UI | your `web/index.html`, shown in a full-screen WebView (phone CSS added) |
| Engine | your `downloader_core.py` + `server.py` (Flask + yt-dlp) running **inside the app** via Chaquopy |
| Android glue | `android_main.py` + Kotlin: save to Gallery, notifications, share-sheet, background service |

Downloads are saved to **Movies/Shaheen Video Downloader** (MP3/audio: **Music/...**), so
they appear in the Gallery. No storage permission is needed on Android 10+.

## Get the APK (no Android Studio needed)

1. Create a free GitHub repository and upload this whole folder.
2. Open the **Actions** tab, run **Build Android APK** (or just push).
3. After about 10 minutes download **Shaheen-Video-Downloader-APK** from the run page, unzip, and install the .apk
   (allow "install unknown apps" for your browser/file manager when asked).

Or in Android Studio: open this folder, let it sync, then **Build > Build APK(s)**.

### Keep updates installable (recommended)
Create one keystore and store it as GitHub secrets, so every new APK is signed with the same key:
`KEYSTORE_BASE64`, `KEYSTORE_PASSWORD`, `KEY_ALIAS`, `KEY_PASSWORD`.
Without them each build is signed with a temporary key and must be uninstalled before installing the next one.

### ffmpeg (needed for MP3 and for 1080p/4K merging)
Phones have no ffmpeg. Put an Android build of it at
`app/src/main/jniLibs/<abi>/libffmpeg.so` (arm64-v8a, armeabi-v7a, x86_64),
or set the repository variable `FFMPEG_ANDROID_URL` to a zip of those folders.
Without ffmpeg the app still runs: MP4 downloads use a single already-merged stream (usually up to 720p on YouTube), MP3 is
disabled with a clear message.

## Using it
* Paste a link, or use **Share > Shaheen Video Downloader** from YouTube, Instagram, TikTok, Facebook, X, Vimeo, any browser.
* Progress shows in a notification and downloads keep running with the screen off.
* The engine (yt-dlp) checks for a newer version every 12 h and applies it on the next launch, so sites keep working without a new APK.

## What changed vs. desktop
* Removed: folder picker, floating progress window, system tray, browser-extension bridge (all desktop-only).
* The local API is protected by a per-launch secret, so other apps cannot control the downloader.
* The desktop "re-encode to H.264" step is skipped (phones play VP9/AV1 natively).
