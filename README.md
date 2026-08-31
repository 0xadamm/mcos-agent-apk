# MCOS Device Agent — APK

Signed release build of the MediCube MCOS device agent (`com.medicube.mcos.agent`) for TCN/Yunshu Android vending boards.

This repo holds **one file: the current APK**. It exists so a machine in the field can download the agent directly over its own browser — no adb, no USB drive.

## Current build

| | |
|---|---|
| File | `mcos-device-agent-v1.3.0-api25.apk` |
| Version | `1.3.0-api25` (versionCode 4) |
| Package | `com.medicube.mcos.agent` |
| minSdk / targetSdk | 24 / 28 |
| Signing | v2 scheme · `CN=MediCube Device Agent, O=Cube Wellness Technologies` |
| SHA-256 | `b73ce36eb088a23e69262c7dc166bc62022ce8879b97f1c36852c41c1776961c` |
| Built | 2026-08-24 |

Verify after download:

```bash
shasum -a 256 mcos-device-agent-v1.3.0-api25.apk
```

## Install on a machine

1. Break out of the YS kiosk with the admin gesture + PIN to reach the engineering/service menu.
2. Use **"Go to ES browser"** from that menu — the full ES File Explorer. (Opening ES through YS's normal content flow gives only a media picker.)
3. Enable **Unknown sources** (Android 7.x: Settings → Security. 8.x+: per-app "Install unknown apps" for the downloading app.)
4. Download the APK from this repo's raw URL, then tap it → Install.
5. Tap the **MCOS** launcher icon once so the app creates its files directory.
6. Copy `bootstrap<machineId>.json` into `/sdcard/Android/data/com.medicube.mcos.agent/files/` and rename it to exactly **`bootstrap.json`** — the app ignores any other name, and secure-deletes it after import.
7. Reopen the app. It provisions and connects to `wss://agent.medicubehealth.net/device/v1`.
8. **Verify on the server**, not the device. The on-device status screen confirms "provisioned" only — it does not prove the socket opened.

Bootstrap files are per-machine secrets and are **not** in this repo.

## Updating this repo

Replace the APK with the new build and update the table above. Every build must be signed with the same keystore or Android will reject the update on every deployed machine — the keystore lives outside this repo and is never committed.

Source, build notes, and toolchain: the `Medicube-MCOS` / `medicube-v2` working repos.
