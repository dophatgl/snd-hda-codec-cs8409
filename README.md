

#This answer is based on similar solutions which work in Debian.

First install kernel-headers, and tools for compiling.

sudo apt install linux-headers-generic
sudo apt install build-essential git gcc-12

Download the driver

git clone https://github.com/egorenar/snd-hda-codec-cs8409

Finally, compile and install the driver
```
cd snd-hda-codec-cs8409
make
sudo make install
```
Reboot the computer and sound should work.

#Ref
https://askubuntu.com/questions/1510298/dummy-output-and-no-input-devices-on-2017-imac-pro


#workaround
If you are seeing two audio devices on your Arch Linux system and facing sound issues, here are the steps to troubleshoot and fix the problem:

---

### **1. Identify Audio Devices**
Use the following command to list the available audio devices and verify their names:
```bash
aplay -l
```
This will display a list of all detected audio devices. Identify the device that corresponds to your intended output (e.g., built-in speakers, external speakers, HDMI, etc.).

---

### **2. Check Default Audio Device**
Run the following command to see the current default audio device:
```bash
cat /proc/asound/card*/id
```
If the default device is incorrect, you can change it by editing the ALSA configuration file.

---

### **3. Set the Correct Default Device**
Edit the ALSA configuration file to set the default device. Create or edit the file:
```bash
sudo nano /etc/asound.conf
```
Add the following lines:
```plaintext
defaults.pcm.card X
defaults.ctl.card X
```
Replace `X` with the card number of the desired audio device found in step 1.

Save and exit the editor, then restart ALSA:
```bash
sudo systemctl restart alsa
```

---

### **4. Use `pavucontrol` for PulseAudio**
If you are using PulseAudio, install `pavucontrol` to manage and switch between audio devices:
```bash
sudo pacman -S pavucontrol
```
Open the PulseAudio Volume Control:
```bash
pavucontrol
```
- Go to the **Configuration** tab to disable the unused device.
- In the **Output Devices** tab, select the preferred device as the fallback.

---

### **5. Check `alsamixer` Settings**
Launch `alsamixer` to check if the devices are muted:
```bash
alsamixer
```
- Use the arrow keys to navigate and adjust the volume.
- Press `F6` to select a specific sound card.
- Ensure "Master" and the relevant device are unmuted (look for "MM" indicating mute, toggle with `M`).

---

### **6. Restart Audio Services**
Restart your audio services to ensure changes take effect:
```bash
sudo systemctl restart pipewire.service
# or if using PulseAudio:
pulseaudio -k && pulseaudio --start
```

---

### **7. Test Audio**
Test the audio output using:
```bash
speaker-test -t wav -c 2
```
Or play a sound file:
```bash
aplay /usr/share/sounds/alsa/Front_Center.wav
```

---

### **8. Advanced: Blacklist Unwanted Audio Devices**
If you do not want one of the devices to appear, blacklist its kernel module. Find the module name using:
```bash
lspci -v | grep -A7 -i audio
```
Then blacklist it by adding it to `/etc/modprobe.d/blacklist.conf`:
```plaintext
blacklist module_name
```
Replace `module_name` with the correct name, then reboot.

---

If these steps don't resolve the issue, let me know more specifics about the devices you're seeing and your setup.
