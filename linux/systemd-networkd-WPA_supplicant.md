# How to Set Up Wi-Fi with These Two Bad Boys

1. `networkctl list` to identify wifi interface name. Should be something like wlan0 or similar.
   > supposed to be with `ip link` but eh...
2. `wpa_passphrase "[SSID]" "[password]" > /etc/wpa_supplicant/wpa_supplicant-[interface-name].conf` You actually define password here.
   > How do i identify/search for wifi?
3. make networkd configuration at `/etc/systemd/network/25-wifi.network`.
   It looks like this:
   ```
   [Match]
   Name=[interface-name]
   
   [Network]
   DHCP=yes # isnt strictly this, but this simply works
   ```
4. `systemctl start systemd-networkd`
5. `systemctl start wpa_supplicant@[interface-name]` to finally connect. 
6. To check if the connection succeeded:
   - `ping 1.1.1.1`. `ping google.com` shouldn't work yet (as handled in next step)
   - read the logs at `systemctl status wpa_supplicant@[interface-name]`.
   > failed connections (e.g. wrong psk/password) still show as `active`.
7. Fix the DNS (unresolvable domain names).
   My specific case is a poor case of completely missing `systemd-resolved`. I don't know how to use that, but just in case we're in the same boat: Edit `/etc/resolv.conf`.
   you can simply add:
   ```
   nameserver 1.1.1.1 # which is cloudflare's nameserver
   ```
8. Enjoy!

IF you face a problem that isn't listed here... well, idk about it!