LINUX TODO:
- configure wine/bottles - LATER
- dont insta shutdown on power button, just ask
- optimize startup (but systemd-analyze counts the time i take to input words)
- remove the dang plop plop sound

SWAY TODO:
- clipboard
- notif bar
- session management menu (ctrl alt del) (probably not)
- swaylock-effects

THINGS I CAN LEARN:
conceptual
- POK stuff, ofc
- transistors microchips
- wireless networks, wpa wpsk, wlan...
- wtf is a socket???
practical
- review "Learning Linux Layers" in GPT (a curriculum)
- networking architecture, nmcli nmtui iwd wpa_supplicant systemd_resolved
- font config (fc-something)
- how to use latex
- curses

Migration List:
- Browsers
  Firefox, Waterfox (gradient blue and red), + lynx

- Work
  codium
+steam
+inputremapper
+btop

forgot conserve.sh
#!/bin/sh

PATH_SYS="/sys/bus/platform/drivers/ideapad_acpi/VPC2004:00/conservation_mode"

current=$(cat "$PATH_SYS")

if [ "$current" = "1" ]; then
    echo 0 | sudo tee "$PATH_SYS" > /dev/null
    notify-send "Battery Conservation" "Disabled" -t 3000
else
    echo 1 | sudo tee "$PATH_SYS" > /dev/null
    notify-send "Battery Conservation" "Enabled (≈60%)" -t 3000
fi
