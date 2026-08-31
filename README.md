# Garlic-Player on Xubuntu 26.04 LTS

This guide explains how to install **Garlic-Player** on **Xubuntu 26.04 LTS**, connect it to **Garlic-Hub**, configure fullscreen mode, and automatically start the player after the computer boots.

## Requirements

* Xubuntu 26.04 LTS
* Internet connection
* A running Garlic-Hub server
* Garlic-Player AppImage
* A dedicated Linux user account for the signage player

---

## 1. Update Xubuntu

Open a terminal and update the system:

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 2. Install AppImage / FUSE Support

Garlic-Player is distributed as an AppImage. Install the required FUSE library:

```bash
sudo apt install -y libfuse2t64
```

---

## 3. Download Garlic-Player

```bash
https://garlic-signage.com/downloads/
```
Garlic-Player provides players for different platforms. This guide uses the **Linux x64 AppImage**.

Download the Linux player:

```bash
wget https://garlic-signage.com/downloads/garlic-player-x64.AppImage
```

Make the AppImage executable:

```bash
chmod +x ~/garlic-player-x64.AppImage
```

You can verify that it exists:

```bash
ls -lh ~/garlic-player-x64.AppImage
```

---

## 4. Start Garlic-Player

Before configuring automatic startup, test the player manually:

```bash
~/garlic-player-x64.AppImage
```

Garlic-Player should open normally.

---

## 5. Configure Garlic-Hub

When Garlic-Player starts, enter the **Garlic-Hub Content URL**.

For example, if your Garlic-Hub server is running at:

```text
192.168.29.100
```

and Docker exposes Garlic-Hub on port `8090`, use:

```text
http://192.168.29.100:8090/smil-index
```

Replace `192.168.29.100` with the IP address of your Garlic-Hub server.

### Garlic-Hub URLs

**Content URL:**

```text
http://YOUR-SERVER-IP:8090/smil-index
```

**Garlic-Hub Web Interface:**

```text
http://YOUR-SERVER-IP:8090
```

---

## 6. Register the Player

Once Garlic-Player connects to the Garlic-Hub content URL, open Garlic-Hub in a web browser:

```text
http://YOUR-SERVER-IP:8090
```

Go to:

**Player Overview**

Your Garlic-Player should appear there.

Complete the registration/configuration from the Garlic-Hub interface as required.

---

## 7. Test Fullscreen Mode

For a dedicated digital signage machine, run Garlic-Player in fullscreen mode.

The following command has been tested successfully:

```bash
~/garlic-player-x64.AppImage --windows-mode fullscreen
```

Run it:

```bash
~/garlic-player-x64.AppImage --windows-mode fullscreen
```

Garlic-Player should start in fullscreen mode.

> **Tip:** Test fullscreen mode manually before configuring automatic startup. This makes troubleshooting much easier.

---

## 8. Configure Automatic Startup

Xubuntu uses XFCE's autostart mechanism.

Create the autostart directory:

```bash
mkdir -p ~/.config/autostart
```

Create the Garlic-Player desktop entry:

```bash
nano ~/.config/autostart/garlic-player.desktop
```

Add the following:

```ini
[Desktop Entry]
Type=Application
Name=Garlic Player
Comment=Garlic Digital Signage Player
Exec=/home/mahesh/garlic-player-x64.AppImage --windows-mode fullscreen
Terminal=false
StartupNotify=false
X-GNOME-Autostart-enabled=true
```

Save the file.

### Verify the autostart configuration

```bash
cat ~/.config/autostart/garlic-player.desktop
```

You should see:

```ini
[Desktop Entry]
Type=Application
Name=Garlic Player
Comment=Garlic Digital Signage Player
Exec=/home/mahesh/garlic-player-x64.AppImage --windows-mode fullscreen
Terminal=false
StartupNotify=false
X-GNOME-Autostart-enabled=true
```

> **Important:** If your Linux username is not `mahesh`, change `/home/mahesh/` to your actual home directory.

You can find your username with:

```bash
whoami
```

---

## 9. Enable Automatic Login

For a dedicated digital signage computer, automatic login is useful because the system can start Garlic-Player automatically after a power failure or reboot.

Edit the LightDM configuration:

```bash
sudo nano /etc/lightdm/lightdm.conf
```

Add:

```ini
[Seat:*]
autologin-user=mahesh
autologin-user-timeout=0
```

Replace `mahesh` with your actual Linux username if necessary.

Save the file.

---

## 10. Reboot and Test

Reboot the Xubuntu machine:

```bash
sudo reboot
```

After reboot, the expected sequence is:

```text
Power On
   ↓
Xubuntu boots
   ↓
Automatic login
   ↓
XFCE desktop starts
   ↓
Garlic-Player starts automatically
   ↓
Garlic-Player opens fullscreen
   ↓
Garlic-Hub content is displayed
```

Your Xubuntu machine can now operate as a **dedicated Garlic digital signage player**.

---

## 11. Troubleshooting

### Garlic-Player does not start

Try launching it manually:

```bash
~/garlic-player-x64.AppImage
```

If that works, test fullscreen:

```bash
~/garlic-player-x64.AppImage --windows-mode fullscreen
```

---

### Permission denied

Make the AppImage executable again:

```bash
chmod +x ~/garlic-player-x64.AppImage
```

Then test:

```bash
~/garlic-player-x64.AppImage
```

---

### AppImage / FUSE error

Make sure the required FUSE package is installed:

```bash
sudo apt install -y libfuse2t64
```

---

### Garlic-Player does not start after login

Check the autostart file:

```bash
cat ~/.config/autostart/garlic-player.desktop
```

Check that the AppImage exists:

```bash
ls -lh ~/garlic-player-x64.AppImage
```

Check the executable permission:

```bash
ls -l ~/garlic-player-x64.AppImage
```

The file should have executable permissions, such as:

```text
-rwxr-xr-x
```

You can also test the exact command from the terminal:

```bash
/home/mahesh/garlic-player-x64.AppImage --windows-mode fullscreen
```

---

### Garlic-Player cannot connect to Garlic-Hub

First test whether Garlic-Hub is reachable from the signage computer:

```bash
curl http://192.168.29.100:8090
```

Replace the IP address with your Garlic-Hub server address.

Also verify that the content URL is:

```text
http://YOUR-SERVER-IP:8090/smil-index
```

and that Garlic-Hub is running.

---

## 12. Useful Commands

### Start normally

```bash
~/garlic-player-x64.AppImage
```

### Start fullscreen

```bash
~/garlic-player-x64.AppImage --windows-mode fullscreen
```

### Check AppImage

```bash
ls -lh ~/garlic-player-x64.AppImage
```

### Check permissions

```bash
ls -l ~/garlic-player-x64.AppImage
```

### Check autostart configuration

```bash
cat ~/.config/autostart/garlic-player.desktop
```

### Check current username

```bash
whoami
```

### Reboot

```bash
sudo reboot
```

---

## 13. Final Configuration

The final setup should look like this:

```text
Xubuntu 26.04 LTS
       │
       ├── Automatic Login: mahesh
       │
       ├── XFCE Autostart
       │       │
       │       └── Garlic-Player
       │              │
       │              └── Fullscreen
       │
       └── Garlic-Hub
              │
              └── http://SERVER-IP:8090/smil-index
```

### Garlic-Player command

```bash
/home/mahesh/garlic-player-x64.AppImage --windows-mode fullscreen
```

### Garlic-Hub Content URL

```text
http://YOUR-SERVER-IP:8090/smil-index
```

### Garlic-Hub Web Interface

```text
http://YOUR-SERVER-IP:8090
```
