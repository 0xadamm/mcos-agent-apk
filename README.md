# MCOS Device Agent — APK

Signed release build of the MediCube MCOS device agent (`com.medicube.mcos.agent`) for TCN/Yunshu Android vending boards.

This repo holds **one file: the current APK**. It exists so the agent can be installed in the field with no adb — either downloaded straight to the machine over its own browser, or carried in on a USB thumb drive.

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

`adb` does not work on these boards — the exposed USB ports are host/power mode and network adb is closed. Both install paths below go through ES File Explorer instead.

### Step 1 — reach ES File Explorer (both paths)

1. Break out of the YS kiosk with the admin **gesture + PIN** to reach the engineering/service menu.
2. Use the **"Go to ES browser"** button in that menu. It launches the full ES File Explorer. Opening ES through YS's normal content flow gives only a gallery/music media picker, which cannot install anything.
3. Turn off **"Keep APP working foreground"** in the engineering menu so YS stops pulling itself back to the front. This is temporary — it re-enables itself after roughly 15 minutes, so work in that window.
4. Enable **Unknown sources**: Android 7.x → Settings → Security → checkbox. Android 8.x+ → per-app "Install unknown apps" for ES (or for the browser, if downloading). Reach Settings via **"Go to Android setting"** in the same engineering menu.

### Step 2a — install from a USB thumb drive

Use this when the machine has no usable network, or you are provisioning several machines in one visit.

**Prepare the drive before you leave:**

1. Format the drive **FAT32**. exFAT and NTFS are not reliably mounted by these boards.
2. Copy the APK to the root of the drive:
   ```bash
   cp mcos-device-agent-v1.3.0-api25.apk /Volumes/<DRIVE>/
   ```
3. Copy the machine's `bootstrap<machineId>.json` alongside it. Carry one per machine you plan to visit, and keep the machine ID in the filename so they do not get mixed up. **Leave them named per-machine on the drive** — the rename to `bootstrap.json` happens on the device, in step 6 below.
4. Eject the drive properly so the write flushes.

**At the machine:**

5. Plug the drive into a USB port on the board and open ES File Explorer. The drive appears as **usb** in the sidebar, or browse to `/mnt/usb_storage`. If it does not appear within a few seconds, unplug and try another port.
6. Tap the APK → **Install**.
7. Tap the **MCOS** launcher icon once so the app creates its files directory.
8. Back in ES, copy the machine's `bootstrap<machineId>.json` from the drive into `/sdcard/Android/data/com.medicube.mcos.agent/files/` and **rename it to exactly `bootstrap.json`**. The app ignores any other name. It secure-deletes the file after import, so nothing is left behind on the device.
9. Continue to step 3 below.

### Step 2b — install by download

Use this when the machine has working network.

5. In ES File Explorer or the board's browser, open this repo's raw APK URL and download it.
6. Tap the downloaded APK → **Install**.
7. Tap the **MCOS** launcher icon once so the app creates its files directory.
8. Get `bootstrap<machineId>.json` onto the device (USB drive is still the safest carrier for it — it is a secret, do not email or paste it), copy it into `/sdcard/Android/data/com.medicube.mcos.agent/files/`, and **rename it to exactly `bootstrap.json`**.

### Step 3 — provision and verify

9. Reopen the app. It imports the bootstrap file and connects to `wss://agent.medicubehealth.net/device/v1`.
10. **Verify on the server, not the device.** The on-device status screen confirms "provisioned" only — it does not prove the socket opened.
11. Re-enable **"Keep APP working foreground"** and confirm YS is back in front and able to vend before you leave the machine.

### Do not

- **Never disable, force-stop, or uninstall the YS / `com.tcn.vending` package.** Without adb it is very hard to undo, and YS is what actually vends. Changing its default-Home flag is reversible; disabling it is not.
- Do not commit or share `bootstrap*.json`. They are per-machine secrets, they are **not** in this repo, and `.gitignore` here blocks them.

## Updating this repo

Replace the APK with the new build and update the table above. Every build must be signed with the same keystore or Android will reject the update on every deployed machine — the keystore lives outside this repo and is never committed.

Source, build notes, and toolchain: the `Medicube-MCOS` / `medicube-v2` working repos.
