# rtl8188eu driver — fixed for kernel 6.15+ / Kali 7.0.12

Fork of aircrack-ng/lwfinger rtl8188eu, patched for kernels where legacy timer API was removed.

## Changes
- Makefile: `EXTRA_CFLAGS += -I$(src)/include` → `ccflags-y += -I$(src)/include` (EXTRA_CFLAGS deprecated/removed in kbuild)
- include/osdep_service.h: `del_timer_sync()` → `timer_delete_sync()` (renamed in kernel 6.15)
- core/rtw_led.c: `from_timer()` → `timer_container_of()` (renamed in kernel 6.16)

## Install
\`\`\`bash
git clone https://github.com/<username>/rtl8188eu-kali-7.0-fix.git
cd rtl8188eu-kali-7.0-fix
sudo dkms add .
sudo dkms build -m rtl8188eu -v 4.1.4
sudo dkms install -m rtl8188eu -v 4.1.4
sudo modprobe -r rtl8xxxu
sudo modprobe 8188eu
\`\`\`

**Важно:** заблокируй встроенный rtl8xxxu, иначе он перехватит устройство:
\`\`\`bash
echo "blacklist rtl8xxxu" | sudo tee -a /etc/modprobe.d/blacklist-rtl.conf
echo "install rtl8xxxu /bin/false" | sudo tee -a /etc/modprobe.d/blacklist-rtl.conf
sudo update-initramfs -u
\`\`\`

rtl8188eu
=========

Repository for the stand-alone RTL8188EU driver.

Compiling & Building
---------
### Dependencies
To compile the driver, you need to have make and a compiler installed. In addition,
you must have the kernel headers installed. If you do not understand what this means,
consult your distro.
### Compiling

> make all

### Installing

> sudo make install

Submitting Issues
---------

Frequently asked Questions
---------

### The network manager says: "Device is not ready"!
Make sure you copied the firmware (rtl8188eufw.bin) to /lib/firmware/rtlwifi/

### NetworkManager does not list SSID
NetworkManager changes the Wi-Fi MAC address during scanning to improve privacy but this adapter does not support it. To address this issue, please create `/etc/NetworkManager/conf.d/80-wifi.conf` with content:

```
[device]
wifi.scan-rand-mac-address=no
```

and run `systemctl restart NetworkManager`
