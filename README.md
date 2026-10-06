# Morrowind VR Quest 2

Standalone OpenMW VR for Meta Quest 2. This repository mirrors the **v0.51** source and the unmodified prebuilt APK from [tomups/openmw-vr-quest](https://github.com/tomups/openmw-vr-quest).

## Download the app

[Download OpenMW VR v0.51 APK](https://github.com/SmashAtoms/MorrowindVRQuest2/raw/refs/heads/main/apk/OpenMW-VR-Quest-v0.51.apk)

- Android package: `org.openmw.vr`
- App version: `0.51-vr`
- APK size: 86,476,642 bytes
- SHA-256: `6ac4563a13be1a6c3674d35f5174317991b84373b405ae4ec0cade2f4c6f0960`
- Original release: https://github.com/tomups/openmw-vr-quest/releases/tag/v0.51
- Source snapshot: [`fd4de49c62f91f829927f59a017a54256720e9ea`](https://github.com/tomups/openmw-vr-quest/tree/fd4de49c62f91f829927f59a017a54256720e9ea)

The APK was copied from the upstream release and verified against its published SHA-256. It was not rebuilt or modified for this repository. The engine source and original build scripts are included below; this import does not include the upstream Git history.

## Install

Enable developer mode on the Quest, connect it to ADB, and accept the USB debugging prompt. Install with SideQuest, or run:

```sh
adb install -r apk/OpenMW-VR-Quest-v0.51.apk
```

You need your own copy of Morrowind and its game data. The source and APK here contain no commercial Morrowind game data or personal saves. See the upstream instructions below for copying your data and building the app.

## Known issue on Quest 2

A crash was observed with this build after leaving Balmora's Fighters Guild for the exterior. The game log reported `RigGeometry rendering with no skeleton`, followed about 17 seconds later by a native `SIGSEGV / SEGV_ACCERR` crash. The available backtrace does not identify the exact cause, and the rendering errors are not proven to be the cause. No crash fix is included in this mirror.

## Credits and license

All engine and port credit belongs to OpenMW, OpenMW-VR, tomups, and their contributors. See [AUTHORS.md](AUTHORS.md), [LICENSE](LICENSE), and the retained third-party notices for attribution and license terms. The upstream GPL v3 license is preserved.

## Original upstream README

# OpenMW-VR for Meta Quest (native APK)

Self-contained build of OpenMW-VR as a standalone Quest APK (OpenXR + GLES via gl4es).
Everything needed lives in this repo:

```
android/
  buildscripts/   cross-compiles the engine + all native deps for arm64-v8a
  app/            minimal VR APK that loads libopenmw.so straight into VR
```

Based on https://gitlab.com/madsbuvi/openmw/-/tree/openmw-51 and https://github.com/xyzz/openmw-android

## Game data

The APK ships no Morrowind data. Put your Morrowind install in **either** location:

```sh
# Option A — full Morrowind folder on shared storage (contains "Data Files/"):
adb push Morrowind/ /sdcard/Morrowind/
# Option B — app-scoped folder, just the Data Files contents (no permission needed):
adb push "Morrowind/Data Files/." /sdcard/Android/data/org.openmw.vr/files/data/
```

`OpenMWActivity` regenerates `…/files/config/openmw.cfg` on each launch, scanning
`/sdcard/Morrowind/Data Files`, `/sdcard/Morrowind`, and the app-scoped `data/` folder — it adds a
`data=` line for each that exists and a `content=` entry per game file found (Morrowind/Tribunal/
Bloodmoon ordered first). For Option A, grant the storage permission when prompted, or pre-grant:

```sh
adb shell pm grant org.openmw.vr android.permission.READ_EXTERNAL_STORAGE
```

## Install & run on the headset (one command)

Once the Quest is reachable over adb (USB, or `adb connect <ip>:5555`):

```sh
android/run-on-device.sh ~/Morrowind   # pushes data, installs, grants perms, launches, tails log
android/run-on-device.sh               # if data is already on the device
```

It captures the startup log to `android/omw-device.log` (filtered to OpenMW/SDL/OpenXR/crash tags).

## Build

```sh
cd android/buildscripts
./build.sh            # downloads the NDK, builds deps + libopenmw.so, deploys into app/
cd ..
ANDROID_HOME=$HOME/Android/Sdk ./gradlew :app:assembleDebug
adb install -r app/build/outputs/apk/debug/app-debug.apk
```


## Debugging

`build.sh` keeps unstripped libs in `buildscripts/symbols/arm64-v8a/` for symbolizing
tombstones. Watch startup with:

```sh
adb logcat -s OpenMW SDL openxr VrApi DEBUG
```
