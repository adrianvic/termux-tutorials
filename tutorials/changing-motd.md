# Changing welcome message (MOTD)
Every time you start a new session, Termux will show you a welcome message - often referred as *MOTD* (message of the day) -, but the default message can get boring after a few times. However the login message is a script that can be changed, that's what you're going to see in this tutorial.

## Editing the script
You can find `motd.sh` in the directory `/data/data/com.termux/files/usr/etc/profile.d/motd.sh`, that's the script responsible for showing the MOTD.

After ensuring the file is there, you can open it with any text editor (I'll be using, for instance, *Vim*). For ease of access you can create an alias in your `.bashrc`, append this to yours and access it by typing `termux-intro`. Don't forget to change `vim` to your preferred text editor:

```bash
alias termux-intro="vim /data/data/com.termux/files/usr/etc/profile.d/motd.sh"
```

Feel free to remove the original content of this file. Just make sure the first line is `#!/usr/bin/bash`.

## Making your custom script

The file is a *bash script*, therefore you cannot just type what you want to be shown, that also means you can run commands. For outputting plain text, you can use the command `echo`:

```bash
echo Welcome to my super duper server!
echo Please give a star on GitHub ;)
```

But - of course - you can do cooler stuff, like running `fastfetch` on startup:

```bash
# Shows no logo for cleaner MOTD, you can remove '--logo none' to show your OS logo.
fastfetch --logo none
```

This will output something like:

```bash
u0_a411@localhost
-----------------
OS: Android REL 13 aarch64
Host: samsung SM-A307GT
Kernel: Linux 4.4.302-p6
Uptime: 1 day, 2 hours, 20 mins
Packages: 175 (dpkg)
Shell: bash 5.3.3
Terminal: dropbear
CPU: 2 x exynos7885 (8) @ 2.08 GHz
GPU: Mali-G71 [Integrated]
Memory: 1.92 GiB / 3.63 GiB (53%)
Swap: 46.50 MiB / 1.51 GiB (3%)
Disk (/): 1.85 GiB / 4.61 GiB (40%) - ext4 [Read-only]
Disk (/storage/emulated): 16.44 GiB / 50.71 GiB (32%) - fuse
Local IP (tun0): 676.76.767.676/67
Local IP (wlan0): 192.168.18.87/24 *
Battery: 100% [AC Connected]
```

Another example is outputting device info from _termux-api_, which must be installed both with `pkg install termux-api` and as an extension from the app store you originally downloaded Termux. See the example for showing the battery temperature when logging in:

```bash
# We're parsing the result of 'termux-battery-status' with 'jq' and printing that with 'echo'.
echo Battery temperature `termux-battery-status | jq .temperature`°C
```

And you can make cool ASCII arts using the package `figlet`:

```bash
figlet foobar
```

Will output:

```
  __             _
 / _| ___   ___ | |__   __ _ _ __
| |_ / _ \ / _ \| '_ \ / _` | '__|
|  _| (_) | (_) | |_) | (_| | |
|_|  \___/ \___/|_.__/ \__,_|_|
```

After saving the file, you can start a new session or SSH into your server to see the new MOTD.
