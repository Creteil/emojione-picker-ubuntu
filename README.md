# Emojione Picker for Ubuntu

Emojione-picker-Ubuntu is a simple emoji picker for Ubuntu 14.04+ running Unity 7 and some other GNU/Linux distributions and desktop environments.

![Image of Emoji-picker](https://raw.githubusercontent.com/gentakojima/emojione-picker-ubuntu/master/screenshot.png)

This project is proudly using emoji data from the [Emoji One](http://emojione.com/) project.

> **About this fork:** the [original project](https://github.com/gentakojima/emojione-picker-ubuntu) targets Python 2.7, which is no longer available on current distributions. This fork ports it to **Python 3** (PyGObject, Ayatana or legacy AppIndicator) and modernizes the Debian packaging (debhelper 13, dh-python, icons shipped in the package). It has been tested on Ubuntu 22.04 with Unity 7.

## Installation

Installation is optional, but recommended to run the program at session startup easily. 

### Method 1: Install Debian Package from PPA

> ⚠️ The PPA ships the original **Python 2** version and only covers Ubuntu 14.04, 15.10 and 16.04. On a current system, use method 2 or 3 instead.

  1. Add [Official PPA](https://launchpad.net/~ys/+archive/ubuntu/emojione-picker): `sudo add-apt-repository ppa:ys/emojione-picker`
  2. Reload package indexes: `sudo apt-get update`
  3. Install package: `sudo apt-get install emojione-picker`

### Method 2: Build a Debian Package from source

Building does not require root access (only installing the package does). Tested on Ubuntu 22.04, it should work on any Debian-based distribution shipping Python 3 and debhelper 13.

  1. Install build dependencies: `sudo apt-get install build-essential debhelper dh-python python3 fakeroot git`
  2. Clone the repository and cd into it: `git clone https://github.com/Creteil/emojione-picker-ubuntu; cd emojione-picker-ubuntu`
  3. Build the package: `dpkg-buildpackage -b -us -uc`
  4. Install it! `sudo apt install ../emojione-picker_*_all.deb` (`apt` pulls the runtime dependencies automatically)

### Method 3: Standalone installer (should work on any distro!)

This method does not require root access. If you run it as root, the picker will be installed for all users.

  1. Install runtime dependencies: `sudo apt-get install python3-gi gir1.2-gtk-3.0 gir1.2-notify-0.7 gir1.2-ayatanaappindicator3-0.1 librsvg2-common xdotool` (use `gir1.2-appindicator3-0.1` instead of the Ayatana package on older releases)
  2. Clone the repository and cd into it: `git clone https://github.com/Creteil/emojione-picker-ubuntu; cd emojione-picker-ubuntu`
  3. Run `./install.sh`
  4. If you want to uninstall it later, just run `./install.sh` again.

You can also try it without installing anything: once the runtime dependencies are installed, just run `./emojione-picker` from the cloned directory.

## How to run it

If you installed it, the picker should be started on a fresh session start. Just close your current session, and start it again.

If you want to manually run it right now, just press `Alt+F2` and write the command `emojione-picker`. If you used the standalone installer and installed it just for the current user, you might need to specify the path to the program, that is `~/.local/bin/emojione-picker`.

## How to use it

To use the picker:

  1. Choose your emoji from the dropdown menu.
  2. Paste it (usually `Ctrl+V`) wherever you want!

You can also enable the **experimental** option to type the emoji instead of using the clipboard in the Settings. If you are interested in this option, please let me know if this is working for you.

The **emoji search window** let you search emojis writing keywords. You can also assign a global hotkey to the command `emojione-picker` and the search window will open on your current mouse position at any time.

## Troubleshooting issues

The indicator usually takes 3 to 5 seconds to load, but it can take up to 30 seconds in older computers. In *Settings*, enable the option *Low end computer mode* to reduce the number of menu items (and emojis). That should speed up loading time. Currently, this mode shows about 1000 emojis out of 1800.

The **emoji search window** might be incorrectly placed under the current active window due to Unity/Compiz focus stealing prevention feature. This is not intended behaviour. The only known workaround is to disable this feature by running this command:

```
dconf write /org/compiz/profiles/unity/plugins/core/focus-prevention-level 0
```

If you want to enable the focus stealing prevention feature again, just run this command instead:

```
dconf write /org/compiz/profiles/unity/plugins/core/focus-prevention-level 1
```

## License

The code is probably poisoned by the GPL somehow, and I'm too lazy to check it. So be it, this will be licensed as GNU GPL v3. You know, *that* license. Check your smart TV or your toaster manual for a printed copy.

Emojis are licensed by the great people of [Emoji One](http://emojione.com/) under a free culture Creative Commons License (CC-BY 4.0).

## FAQ

### Why for Ubuntu only? Does this work with KDE and other distros?

Of course, but it was designed to work with the Unity desktop. It's been reported to work with Cinnamon, XFCE, Openbox (with Stalonetray and Tint2), Gnome Shell, MATE and KDE.

Unfortunately, Cinnamon implementation won't show the content of submenus if the total height is bigger than the screen height.

### When I paste all/some icons, they are shown as ugly boxes. I want my money back!

Recent GNU/Linux distributions (including Ubuntu 14.04, 15.10) won't display emojis properly out of the box.

You can get full color emojis working on Firefox with black and white fallback for other applications following these instructions: https://github.com/eosrei/emojione-color-font

### I get "ValueError: Namespace AppIndicator3 not available" at startup

You're missing the required indicator library. The picker uses Ayatana AppIndicator when available and falls back to the legacy AppIndicator. Please install one of these packages:
  - Debian / Ubuntu 20.04+: `gir1.2-ayatanaappindicator3-0.1`
  - Older Ubuntu: `gir1.2-appindicator3-0.1`
  - OpenSuse Leap: `typelib-1_0-AppIndicator3-0_1`

