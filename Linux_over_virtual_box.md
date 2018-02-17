### Install a virtualizer

For instance VirtualBox, which can be installed through Homebrew:
```
brew cask install virtualbox
```
VirtualBox is an Oravle product, open source and free software and is available on many OSes. Most interesting [wiki](https://www.virtualbox.org/wiki) pages are:
- https://www.virtualbox.org/wiki/Guest_OSes
> Note: Regarding the host OSes, Ubuntu 5.10 is an old release. 6.06 and 6.10 versions have race conditions causing the VM to crash. Prefered Linux OS (good commercial suppor, so should be reliable enough) is Ubuntu 16.04 LTS Xenial Xerus - Kernel 4.4 (support by Ubuntu until 2019 at least; comes with additions).

- [User Manual](https://www.virtualbox.org/manual/) chapters 1-first steps (1.4, 1.7, 1.10, 1.11, 1.13), chapter 3-Configuring virtual machines (3.1, 3.4.1, 3.4.2, 3.7), chapter 4-Guest additions, chapter 8-VBoxManage

### Install Ubuntu on the virtual box

1. Ubuntu desktop wiki pages:
- [Download and requirements](https://www.ubuntu.com/download/desktop)
- install guide(https://tutorials.ubuntu.com/tutorial/tutorial-install-ubuntu-desktop?_ga=2.262926095.2025609413.1516394021-47304954.1516394021#0)
- [Ubuntu Desktop Guide](https://help.ubuntu.com/stable/ubuntu-help/?_ga=2.225693465.2025609413.1516394021-47304954.1516394021) --> Hardware & drivers --> Disks & storage --> Adjust the size of a filesystem

2. Another third party [wiki](https://henricasanova.github.io/VirtualBoxUbuntuHowTo.html) covering virtualbox+linux installation.

3. install commands

- install a package: `apt-get install <package_name>`
- install package dependencies: `apt-get install <package_name>`
- update linux (same distribution): `apt update` then `apt upgrade`

`apt` in most cases equivalent to `apt-get` except that `apt-get` has additional options (for other details, refer to [AptGet/HowTo](https://help.ubuntu.com/community/AptGet/Howto)).

4. Install Atom editor (https://atom.io/)

5. Resize the Ubuntu partitions:
    5.1. with GParted: https://www.howtogeek.com/114503/how-to-resize-your-ubuntu-partitions/
    5.2. with LVM tool using LVM2 volume type: https://wiki.ubuntu.com/Lvm, https://www.howtogeek.com/howto/40702/how-to-manage-and-use-lvm-logical-volume-management-in-ubuntu/

#### Resize partitions with LVM

- Check if you have LVM installed
> apt show lvm2
- Display the current Volume Group, Physical and Logical Volumes
> sudo vgdisplay
> sudo pvdisplay
> sudo lvdisplay
- With the application `Disks`, create a partition with the free space left after the "Extended partition" -> `sda3`
- Create a physical LVM volume
> sudo pvcreate /dev/sda3
- Check full list of physical volumes
> sudo pvdisplay
- Note: only the logical volumes appear under the Volume Group folder
> ll /dev/ubuntu-vg/
- Extend the Volume Group in order to include the newly created Physical Volume
> sudo vgextend ubuntu-vg /dev/sda3
- Extend the `root` logical volume by 5G. It will automatically use the new physical volume available
> sudo lvextend -L +5G /dev/ubuntu-vg/root
- Let the file system account for the extension, by "resizing" the file system to the new size
> sudo resize2fs /dev/ubuntu-vg/root

