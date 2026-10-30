# Hikvision TV App (`com.hiktv.app`)

An Android TV app that shows live and recorded video from a Hikvision DVR/NVR, using the camera's ISAPI (HTTP) and RTSP interfaces.

This repository holds the **installed build (APK)** pulled from the TV. The TV had no source code on it.

## Build info

| Field | Value |
|---|---|
| Package | `com.hiktv.app` |
| Version | 0.1 (versionCode 1) |
| Min / target SDK | 21 (Android 5.0) / 34 (Android 14) |
| Launcher | Android TV (Leanback) and standard launcher |
| Permissions | `INTERNET` only |
| File | `hiktv-0.1.apk` (3.8 MB, 3 dex files, no native libraries) |
| SHA-256 | `d50a9dd7102386e87e9ab44d8ff5fde3626654d422d63dc13468a95f11669efc` |

## Where it came from

| Field | Value |
|---|---|
| Device | Sony BRAVIA 2K GB ATV3, Android TV 9 |
| IP address | 192.168.2.81 (adb over network, port 5555) |
| First installed | 2026-10-28 20:23 |
| Last updated | 2026-10-29 11:26 |
| Pulled | 2026-10-30 |

## What the app does

Inferred from the class names and strings in the APK:

| Class | Role |
|---|---|
| `MainActivity` | Entry point |
| `SetupActivity`, `DvrConfig`, `DvrPrefs` | DVR connection setup (host, ports, login), stored in preferences encrypted with Google Tink |
| `GridActivity` | Multi-camera grid view |
| `PlayerActivity` | Full-screen live view of one camera |
| `PlaybackActivity`, `Recording`, `DvrTime` | Search and play back recordings |
| `RtspClient`, `VideoDecoder`, `AudioPlayer` | Custom RTSP client with hardware video decoding and audio playback |
| `IsapiClient`, `IsapiException` | HTTP client for the Hikvision ISAPI (OkHttp) |
| `CameraRestart` | Restarts a camera stream ("The picture returns in a few seconds") |
| `ColorCheck` | Picture/colour check |

ISAPI endpoints the app uses:

- `/ISAPI/System/deviceInfo`: device information
- `/ISAPI/System/Video/inputs/channels`: list of camera channels
- `/ISAPI/Streaming/channels/<id>`: stream settings
- `/ISAPI/ContentMgmt/search`: recording search
- `/ISAPI/Event/notification/alertStream`: live event/alarm stream

## Install on another Android TV

```bash
adb connect <tv-ip>:5555
adb install -r hiktv-0.1.apk
```

The TV must have **Developer options → USB/Network debugging** turned on. On first launch, enter the DVR's IP address and login in the setup screen.

## Pull a newer build from the TV

```bash
adb connect 192.168.2.81:5555
adb pull "$(adb shell pm path com.hiktv.app | sed 's/package://')" hiktv-<version>.apk
```

## Notes

- No source code is included. To keep developing the app, add the original Android Studio project to this repo.
- The DVR login is stored only on the TV, inside the app's encrypted preferences. It is not in this repository.
