## DISPLAY PROTOCOLS, THEIR SERVERS, Desktop Environments (DE), Window Managers (WM), Display Managers (DM)


# Display protocols
Protocols that define how everything related to what we see is organized. Each has its own architecture.
E.g. X11, Wayland
(Windows: WDDM, MacOS: Quartz)

X11 vs Wayland
X11
-is older
-has been working for a very long time
-a lot of processes rely on its architectural design
-not secure: processes can spy on each other, read input, read screen, etc.
 > they do this through Xorg, which doesnt refuse potentially malicious requests from processes.
Wayland
-newer
-successor (supposed to be) for X11
-people complain because lots of programs (legacy) dont work as most only support X11.
-more secure; sandboxing, each process cant spy on each other, can only see themselves.

you download them separately from DEs.
When you install a DE (or WM), what they provide is support for the certain protocols.
example: KDE Plasma and GNOME fully support both x11 and Wayland, some others have partial support 
(mostly partial for Wayland, since it's new), others are specialized:
-i3 WM for X11
-Sway and Hyprland WM for Wayland

# Display servers
Middleman that organizes requests of applications (as clients), cooperates with the compositor and 
other components of the protocol, and forwards them into the kernel (following the client-server 
[client server] model)

Xorg is the server for X11.
The server for wayland is the compositor itself (directly; no extra overhead, therefore faster
performance)

# Desktop Environment (DE)
Broad term for a full environment/experience for the user.
E.g. KDE Plasma, GNOME, a fully configured WM. 

# Window Manager (WM)
such as i3, Sway, Hyprland.
they usually only provide the windowing system or be the compositor.
they generally come not as full-fledged DEs, but templates, and need to be configured.
that means no login screen, no bars, no network manager...

# Display Manager (DM)
login screen.
E.g. lightdm, sddm, gdm, and my favorite ly.
optionally includes choices for which environment to boot into.
usually comes with the DE, but they are separate from the DE and can be changed easily.
systemctl disable old
systemctl enable new
(setting up ly will explain more)
