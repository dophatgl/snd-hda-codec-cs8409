

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
