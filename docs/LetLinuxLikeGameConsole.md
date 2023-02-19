## :sunny:Set portrait screen orientation and auto launch game when login

1. Create .xprofile file in /home/mkg
2. .xprofile is a shell script, so using "chmod a+x .xprofile" let it be executability
3. Fill down the scripts on below

   ```
   #!/bin/sh
   # Using xrandr to setup porprait dimensions first.
   xrandr --output DisplayPort-1 --off --output DisplayPort-0 --off --output VGA-0 --primary --mode 1920x1080 --pos 0x0 --rotate left

   # Launche game-play
   /home/mkg/Documents/DD/deadroad.x86_64
   ```

## :sunny:Turn off the splash screen of Lubuntu

Edit /etc/default/grub (using gksu gedit /etc/default/grub), and remove the "quiet splash" from the Linux command line. Here's what it looks like by default:

> GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"

Make it look like this:

> GRUB_CMDLINE_LINUX_DEFAULT=""

After this run

> sudo update-grub2


---

<br>

[返回目錄](/README.md)
