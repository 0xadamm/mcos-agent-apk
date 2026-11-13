# MCOS Device Agent — install guide

This page walks you through putting the **MCOS Device Agent** app on a MediCube vending machine. No special tools are needed: a computer, a USB thumb drive, and about 15 minutes at the machine.

The app runs quietly in the background and lets MediCube manage the screen on the machine remotely. It does **not** change how the machine vends.

---

## What you need

|                         |                                                                              |
| ----------------------- | ---------------------------------------------------------------------------- |
| A USB thumb drive       | Any size. It will be erased.                                                 |
| A second USB drive      | For the backup in Part 0. The install drive is erased, so it cannot hold it. |
| A computer              | Windows or Mac, to prepare the drive.                                        |
| The machine's number    | Printed on the machine, usually a 10-digit number such as `2602080991`.      |
| The admin gesture + PIN | Used to open the machine's service menu. Your MediCube contact has these.    |

---

## The short version

1. **Back up** the machine's screens and products to a second USB drive (Part 0).
2. **Prepare the install drive** on a computer: FAT32, the app, and this machine's settings file from MCOS (Part 1).
3. **Put the app in the machine's system folder** with the file manager, set two permissions, and restart (Part 2, steps 5 to 8). Never tap the app and choose Install.
4. **Open the app once**, copy the settings file into the app's folder and rename it `bootstrap.json` (Part 2, steps 9 to 11).
5. **Check it worked** in MCOS before you leave (Part 3).

You do this once per machine. After that, MediCube updates the app remotely (see "Already installed?" at the end).

---

## Part 0: Back up the machine first

Do this before you change anything, including the steps that follow. If the install goes wrong, this copy is the only way back to the machine's current screens and products.

> **Use a second USB drive for the backup.** Part 1 formats the install drive and erases it, so a backup sitting on that drive is gone by step 1 of Part 1.

1. Plug the backup drive into a USB port on the machine's board.
2. On the machine, open the file manager (service menu, then **Go to ES browser**), open the **usb** folder in the drawer, and make a new folder there with **New → Folder**. Name it for the machine and the date, for example `2602080991-2026-10-02`.
3. Copy the **stand-by videos** out of `/storage/emulated/0/TcnFolder/ImageScreen` into the new folder: in the file manager, long-press the folder → **Copy**, then open the new folder on the drive and **Paste**.
4. Copy the **product videos** out of `/storage/emulated/0/TcnFolder/VideoAndImageAd` the same way.
5. Copy the other folders the screen and the products are built from, the same way: `/sdcard/YSConfig`, `/sdcard/TcnFolder/ImageGoods`, and `/sdcard/mcos-backup`.
6. If the machine already has the agent on it, copy its current settings file too: `/sdcard/Android/data/com.medicube.mcos.agent/files/bootstrap.json`.
7. **Confirm on site:** open `/storage/emulated/0/TcnFolder` and `/sdcard` in the file manager and copy anything else you recognise into the backup. The repo records only the folders listed above, so what else is on this machine has to be read at the machine.
8. **Check the copy.** Open the new folder on the drive in the file manager and confirm every file and folder from steps 3 to 6 is in it. **Confirm on site:** open **Properties** on one copied file and compare its size with the original; the repo records no file sizes or counts, so there is nothing to check the numbers against.
9. Unplug the backup drive from the machine. The drive you use for the install is a different one, and Part 1 formats it.

---

## Part 1 — Prepare the USB drive (on your computer)

![Two files go on the USB drive](https://raw.githubusercontent.com/0xadamm/mcos-agent-apk/main/docs/img/1-usb-drive.png)

### 1. Format the drive as FAT32

The machine only reads FAT32 drives.

- **Windows:** right-click the drive → **Format** → File system: **FAT32** → Start.
- **Mac:** open **Disk Utility** → select the drive → **Erase** → Format: **MS-DOS (FAT)** → Erase.

### 2. Download the app

Right-click the link and choose **Save link as…**, then save it to the USB drive.

- **[mcos-device-agent-latest-api25.apk](https://github.com/0xadamm/mcos-agent-apk/releases/latest/download/mcos-device-agent-latest-api25.apk)** — the app itself.

### 3. Get this machine's settings file from MCOS

Each machine needs its own settings file. It carries the machine's number and its private access key, so it has to be made in MCOS, once per machine. You need a MediCube **super admin** login for this step. If yours is a staff login, ask your MediCube contact to do this step and send you the files.

1. In MCOS, open **Machines** and click **Add board**.
2. Type the **machine number** exactly as printed on the machine (for example `2602080991`) and choose the **campus**.
3. Click **Add board**, then click **Download bootstrap-_number_.json**. Save it onto the USB drive next to the APK.

> The file is shown **once**. MCOS keeps only a scrambled fingerprint of the key, so it cannot show it again. If you close the window before downloading, open that machine in MCOS (**Machines** → the machine → **Board access**) and choose **Rotate token** to make a fresh file.
>
> **Rotating a machine that is already installed puts it offline, and there is no remote way back.** The old key stops working at once. Putting the new file on the board means another trip with a USB drive and steps 5 to 11 again. Only rotate a board you are standing at, or one you know is already unreachable.
>
> The file is a secret. Do not email it or post it in chat, and delete it from your computer once the machine is installed.

Do not open or edit the file. Doing several machines in one trip? Add each board in MCOS and put every `bootstrap-<number>.json` on the same drive. At each machine you will copy the matching one and rename it to `bootstrap.json` (step 10).

#### Finding the machine number

You need the machine's number before you can add the board in MCOS (step 3 above). Read it off the board:

- In the file manager, open the **`MachineData`** folder → **`Server`** → long-press **`Machine.txt`** → open it with a text editor. The full path is `/sdcard/MachineData/Server/Machine.txt`.
- The machine number is the **second** field of that file: the text between the first and the second `|`.
- It is usually a 10-digit number such as `2602080991`.
- **No root is needed** to read it.
- The file manager may spell the same volume either way, so the folder can also appear as `storage/emulated/0/MachineData/Server/`. Both spellings resolved to the same volume on the one board this was measured on; the repo has not verified it on other boards.
- **Confirm on site:** if there is no `MachineData` folder, or the second field is not a 10-digit number, look for a printed label on the machine itself carrying the number. The repo does not record where the number is printed.
- If the agent is already on the machine, the **MCOS Device Agent** app shows the machine number on its status screen (step 11), which confirms the number you typed.

### 4. Eject the drive

Use **Safely Remove** (Windows) or **Eject** (Mac) before unplugging, so the files finish writing.

---

## Part 2 — Install on the machine

> **Important: do not tap the APK and choose "Install".** These machines automatically remove apps installed that way within about a minute. The app has to be placed in the machine's system folder instead, and then the machine is restarted. The steps below do exactly that.

![The steps on the machine, in order](https://raw.githubusercontent.com/0xadamm/mcos-agent-apk/main/docs/img/3-on-the-machine.png)

### 5. Open the file manager and turn on Root Explorer

Open the machine's **service menu** (admin gesture + PIN) and tap **"Go to ES browser"**. This opens **ES File Explorer**.

> Use the **"Go to ES browser"** button, not any other way of opening ES. Other routes only open a photo/music picker.

In ES File Explorer:

1. Open the left-hand drawer (the **☰** icon, or swipe in from the left edge) and scroll down to **Tools**.
2. Switch **Root Explorer** to **ON**. If the machine asks to allow root access, allow it.
3. Tap the words **Root Explorer** to open its options, choose **Mount R/W**, and set **/system** to **RW**. Tap OK.

### 6. Copy the app into the system folder

1. Plug the USB drive into a USB port on the machine's board.
2. In ES, go to the machine's top-level folder (in the drawer, tap **Device** or **/**), then open **system**, then **app**.
3. Tap **New → Folder** and name it exactly `MCOSAgent`. Open that new folder.
4. In the drawer tap **usb**, **long-press** `mcos-device-agent-latest-api25.apk` → **Copy**.
5. Go back to **/system/app/MCOSAgent/** and tap **Paste**.
6. **Check the copy landed.** Long-press the pasted APK → **Properties**. It should show a version number. If it shows none, the copy failed silently: delete the file and paste it again before you go on.

If the USB drive does not appear in the drawer within a few seconds, unplug it and try another port.

### 7. Set the permissions

Still in **/system/app/MCOSAgent/**:

1. **Long-press** the APK file → **Properties** → next to Permissions tap **Change**. Tick the boxes so it reads **Owner: Read + Write · Group: Read · Other: Read** (shown as `rw-r--r--`). Tap OK.
2. Go up one level to **/system/app/**, **long-press** the **MCOSAgent** folder → **Properties** → **Change**. Tick so it reads **Owner: Read + Write + Execute · Group: Read + Execute · Other: Read + Execute** (shown as `rwxr-xr-x`). Tap OK.

### 8. Restart the machine

Restart the machine from the service menu, or power it off and on. **Do not tap the APK and choose Install.** The machine installs the app from the system folder while it starts up.

### 9. Open the app once

After the restart, find the new **MCOS Device Agent** icon (it looks like a rounded square with "MC" on it) in the app list and **tap it once**. It will say **"Not provisioned"** in red, with "bootstrap file not found" beside it. That is expected. Opening it once creates the folder you need next. Press back.

### 10. Copy the settings file into the app's folder

![Copy bootstrap.json from the USB drive into the app folder](https://raw.githubusercontent.com/0xadamm/mcos-agent-apk/main/docs/img/4-copy-settings.png)

Open the service menu → **"Go to ES browser"** again if needed. Then in ES File Explorer:

1. Go to **usb** and **long-press** this machine's `bootstrap-<number>.json` (the number must match the machine) → **Copy**.
2. Navigate to this folder: **sdcard → Android → data → com.medicube.mcos.agent → files**
3. Tap **Paste**.
4. Long-press the file you pasted, `bootstrap-<number>.json` → **Rename** → change it to exactly `bootstrap.json`. The app ignores any other name.

### 11. Start the app

Open **MCOS Device Agent** again. The screen should now show **Provisioned: yes** with the machine number. The settings file disappears from the folder on its own once it has been read. That is normal.

---

## Part 3: Check that it worked (before you leave)

"Provisioned: yes" on the machine only means the app read its settings file. It does not prove MediCube can reach the machine. Check both:

1. **Online in MCOS.** Within about a minute, open **Machines** in MCOS. This machine should show as **Online**, with a "last seen" time from the last minute or two. If it does not, check that the number on the machine matches the one you added, and that the file you copied is the one named for this machine.
2. **Survives a restart.** Restart the machine once more from the service menu. It should drop offline and come back **Online** on its own within a few minutes, with nobody touching it. A machine that is only Online right after you opened the app is not finished: go back to step 5 and check the read-write setting held.

The install is done only when check 1 passes.

---

## If something goes wrong

**"Problem parsing the package", or the MCOS Device Agent icon disappears within about two minutes of the restart.** Usually one of three things: the APK was pasted into `/system/app/` instead of inside `/system/app/MCOSAgent/`; the permissions are not exactly as in step 7; or `/system` was not read-write when you pasted. Redo steps 5 to 8, and use the version-number check in step 6 to confirm the copy landed.

**The read-write setting did not survive the restart.** On at least one real install it was off again after a restart. Before any further write to `/system`, open Root Explorer → **Mount R/W** and check **/system** still shows **RW**. Do not assume it held.

**The app still says "Not provisioned" after step 11** (with "bootstrap file not found" beside it). The settings file is not where the app looks. Check it is named exactly `bootstrap.json` and sits in `sdcard/Android/data/com.medicube.mcos.agent/files/`, and that you opened the app once (step 9) before copying it, because that first open creates the folder.

**The app says "Provisioned: yes" but the machine never shows Online in MCOS.** Check, in this order: the wifi signal where the machine stands; that the number on the machine (`Machine.txt`) is the number you added in MCOS; that the file you copied is the one made for this machine. If none of these explains it, stop and send MediCube the machine number and what the screen says. This has happened on a real install without a confirmed cause, so do not guess further.

**You need to set a machine up again.** The settings file cannot be read back off a machine that was already set up. Make a fresh one with **Rotate token** in MCOS (see the warning in Part 1, step 3) and repeat steps 10 and 11.

---

## Already installed? Updates are remote

You do the steps above **once per machine**. After the app is on a machine, MediCube updates it remotely from MCOS: a super admin sees an amber **Out of date** chip on the machine under **Machines** and uses **Update agent**. Do not repeat Part 2 to update an app that is already working.

---

## Please do not

- **Do not uninstall, disable, or force-stop the vending app.** It is what makes the machine vend and it is very hard to restore.
- **Do not change anything in the service menu other than "Go to ES browser" and the restart.**
