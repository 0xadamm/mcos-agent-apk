# MCOS Device Agent — install guide

This page walks you through putting the **MCOS Device Agent** app on a MediCube vending machine. No special tools are needed: a computer, a USB thumb drive, and about 15 minutes at the machine.

The app runs quietly in the background and lets MediCube manage the screen on the machine remotely. It does **not** change how the machine vends.

---

## What you need

| | |
|---|---|
| A USB thumb drive | Any size. It will be erased. |
| A computer | Windows or Mac, to prepare the drive. |
| The machine's number | Printed on the machine, usually a 10-digit number such as `2602080991`. |
| The admin gesture + PIN | Used to open the machine's service menu. Your MediCube contact has these. |

---

## Part 1 — Prepare the USB drive (on your computer)

![Two files go on the USB drive](docs/img/1-usb-drive.png)

### 1. Format the drive as FAT32

The machine only reads FAT32 drives.

- **Windows:** right-click the drive → **Format** → File system: **FAT32** → Start.
- **Mac:** open **Disk Utility** → select the drive → **Erase** → Format: **MS-DOS (FAT)** → Erase.

### 2. Download the two files from this page

Right-click each link and choose **Save link as…**, then save both to the USB drive.

1. **[mcos-device-agent-v1.3.0-api25.apk](https://github.com/0xadamm/mcos-agent-apk/raw/main/mcos-device-agent-v1.3.0-api25.apk)** — the app itself.
2. **[bootstrap.json](https://github.com/0xadamm/mcos-agent-apk/raw/main/bootstrap.json)** — a small settings file that tells the app which machine it is on.

### 3. Type the machine number into `bootstrap.json`

![Replace the placeholder with the machine number](docs/img/2-machine-number.png)

Open `bootstrap.json` **from the USB drive** in a plain text editor:

- **Windows:** right-click the file → **Open with** → **Notepad**.
- **Mac:** right-click the file → **Open With** → **TextEdit**. If TextEdit shows a formatting toolbar, choose **Format → Make Plain Text** first.

The words `REPLACE-WITH-MACHINE-NUMBER` appear **twice**, on these two lines:

```
"machineId": "REPLACE-WITH-MACHINE-NUMBER",
"authenticationToken": "REPLACE-WITH-MACHINE-NUMBER",
```

Replace those words with the machine's number in **both** places. Keep the quotation marks. For machine 2602080991 the two lines become:

```
"machineId": "2602080991",
"authenticationToken": "2602080991",
```

Tip: use **Edit → Replace** (Windows: Ctrl+H, Mac: Cmd+Option+F) to change both at once.

Do not change anything else in the file. Save it, keeping the name `bootstrap.json`.

> Doing several machines in one trip? Make one copy per machine on the drive, named `bootstrap-2602080991.json`, `bootstrap-2602080992.json`, and so on. At each machine you will copy the matching one and rename it to `bootstrap.json` (step 8).

### 4. Eject the drive

Use **Safely Remove** (Windows) or **Eject** (Mac) before unplugging, so the files finish writing.

---

## Part 2 — Install on the machine

### 5. Allow apps to be installed

Open the machine's **service menu** (admin gesture + PIN), tap **"Go to Android setting"**, then:

- Go to **Security** and turn **ON** **"Unknown sources"**. Confirm if asked.
- If there is no "Unknown sources" switch, look for **"Install unknown apps"** instead and allow it for **ES File Explorer**.

Press the back button until you are back in the service menu.

### 6. Plug in the USB drive and open the file manager

![Open ES File Explorer and install the app](docs/img/3-on-the-machine.png)

Plug the drive into a USB port on the machine's board. Then in the service menu tap **"Go to ES browser"**. This opens **ES File Explorer**.

> Use the **"Go to ES browser"** button, not any other way of opening ES. Other routes only open a photo/music picker that cannot install apps.

In ES File Explorer, look for **usb** in the left-hand list and tap it. You should see the two files you copied. If the drive does not appear within a few seconds, unplug it and try a different USB port.

### 7. Install the app

Tap **mcos-device-agent-v1.3.0-api25.apk** → tap **Install** → wait → tap **Done** (not Open).

Now find the new **MCOS Device Agent** icon (it looks like a rounded square with "MC" on it) in the app list and **tap it once**. It will say **"Not provisioned"**. That is expected. Opening it once creates the folder you need next. Press back to return to ES File Explorer.

### 8. Copy the settings file into the app's folder

![Copy bootstrap.json from the USB drive into the app folder](docs/img/4-copy-settings.png)

In ES File Explorer:

1. Go back to **usb** and **long-press** `bootstrap.json` → **Copy**.
2. Navigate to this folder: **sdcard → Android → data → com.medicube.mcos.agent → files**
3. Tap **Paste**.
4. If you used a per-machine name such as `bootstrap-2602080991.json`, long-press it → **Rename** → change it to exactly `bootstrap.json`. The app ignores any other name.

### 9. Start the app

Open **MCOS Device Agent** again. The screen should now show **Provisioned** with the machine number. The settings file disappears from the folder on its own once it has been read. That is normal.

---

## Please do not

- **Do not uninstall, disable, or force-stop the vending app.** It is what makes the machine vend and it is very hard to restore.
- **Do not change anything in the service menu other than the two items above** ("Go to Android setting" and "Go to ES browser").
