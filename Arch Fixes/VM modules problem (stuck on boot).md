## At first look issue:
### Stuck on boot on either 'Reached target user interface' or 'Starting network manager'
#### Other signs:
* [ERR] Failed to load kernel modules (which meant vbox modules) 
* x-server wouldn't start(startx is busy or unreachable)

## Real issue:
### Update broke vm modules somehow? Not sure. But basically GUI just wouldn't start.

## What I did:
- startx (READ THE LOG!!!)
- switch to tty2
- if tty2 blinks change default.target to multi-user.target(to boot to CLI automatically and reboot into that for comfort)

1) Updated vmbox to newer version
2) Tried modprobe -a [list of vm modules] -Failed
3) Tried depmod - Failed
4) Tried installing virtualbox-guest-modules-arch (which was installed prior to upgrade, but somehow wasn't anymore),
pacman responded with a list of files that (like .conf, and kernel modules, which were not supposed to be there)
5) Deleted files listed by pacman in previous step
6) Installed virtualbox-guest-modules-arch - Success
7) Ran depmod - Success(no output)
8) Reboot


## Related Links:
- https://bugs.archlinux.org/task/40495

  depmod should have been run by the install script. (In my case it probably tried, but modules had unrecognizable characters)
- https://unix.stackexchange.com/questions/131792/virtualbox-modprobe-cant-find-vboxguest-vboxsf-vboxvideo
  
  <code>$ depmod 3.14.4-1-ARCH</code>:
  After running modprobe again, it should work.
  You can use uname -r to find your kernel version string.
  
------------------------------------------------
## Conclusions:
* Always check if it's just GUI
* Go for drivers when in doubt (in my case virtualbox stuff)
Spent 1.5 days fixing this.
