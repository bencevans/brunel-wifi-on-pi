# Wifi@Brunel on Pi

## Automatic Setup

Tested on Raspberry Pi OS (32-bit) [Released: 2021-01-11].

1. Load the `connect` script onto the Pi via alternate network, USB, loading onto to SD etc.
2. Open terminal
3. Run `bash path/to/saved/connect`

## Manual Setup

1. Ensure your device MAC addresses are registered on the connect portal. You can get the addresses by running `ip link` in terminal.

2. Stop Networking Services

   ```
   $ sudo systemctl stop networking.service wpa_supplicant.service dhcpcd.service
   ```

3. Setup the econnection profile by adding the following to wpa_supplicant.conf and change 'cs00abc' to your network username and 'networkpassword' to your network password.

   ```
   $ sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
   ```

   ```
   network={
       ssid="Wifi@Brunel"
       eap=PEAP
       identity="cs00abc"
       password="networkpassword"
       phase2="auth=MSCHAPV2"
       pairwise=CCMP TKIP
       key_mgmt=WPA-EAP
   }
   ```

4. Setup the Pi to use an alternative Wifi driver (resolves issues linked in related section bellow). Add the following to the end of `dhcpcd.conf`.

   ```
   $ sudo nano /etc/dhcpcd.conf
   ```

   ```
   interface wlan0
   env ifwireless=1
   env wpa_supplicant_driver=wext
   ```

5. Start network services back up

   ```
    sudo systemctl start networking.service wpa_supplicant.service dhcpcd.service
   ```

## Related

- Issues with default driver impacting EAP connections
  - https://github.com/raspberrypi/linux/issues/3613
  - https://github.com/raspberrypi/linux/issues/3783
