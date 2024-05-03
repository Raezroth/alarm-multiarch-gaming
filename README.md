# Arch Linux Arm Multiarch Gaming

## Experimental

This is an experimental setup for armhf multiarch support on [Arch Linux Arm](https://archlinuxarm.org/) (alarm) using the [ArchLinuxCN](https://github.com/archlinuxcn/repo) repo. Basic programs run currently and a few games. This method still uses a 32bit environment, but only as a runtime folder for 32bit libraries. Some PKGBUILDS are provided for easy of use, others are pulled from their repo.

This guide will require basic terminal usage.

---

### Clone this repo to us a working directory 

```
git clone https://github.com/Raezroth/alarm-multiarch-gaming.git /home/$USER/alarm-multiarch-gaming && cd /home/$USER/alarm-multiarch-gaming
```

---

### Add ArchLinuxCN to your `pacman.conf`

Choose text editor: `export EDITOR=vim` or `export EDITOR=nano` 

Add repo:

```
[archlinuxcn]
Server = https://repo.archlinuxcn.org/$arch
```

to your /etc/pacman.conf. `sudo $EDITOR /etc/pacman.conf`

Then import PGP Keys:

```
sudo pacman -Sy archlinuxcn-keyring
```

Say yes when prompted. This will fail. This is expected.
Due to a developer's retirement, newly installed systems need to manually trust farseerfc's key:

```
sudo pacman-key --lsign-key "farseerfc@archlinux.org"
```

Then run `sudo pacman -Sy archlinuxcn-keyring` again.

---

### Install dependacies

Use your package manager to install dependancies:

```
sudo pacman -S arch-install-scripts arm-linux-gnueabihf-binutils arm-linux-gnueabihf-gcc arm-linux-gnueabihf-gcc-fortran arm-linux-gnueabihf-gcc-libs arm-linux-gnueabihf-gcc-objc arm-linux-gnueabihf-glibc arm-linux-gnueabihf-linux-api-headers
```

---

### Pacstrap a 32bit environment to use as a runtime: (This is for Box86 to run applicatioins beyond itself and Undertale)

Use `pacstrap` from `arch-install-scripts` to setup a 32bit runtime:

```
sudo pacstrap -C ./etc/pacman.conf /home/$USER/.armv7h base base-devel chromium pipewire pipewire-alsa pipewire-pulse
```

Use this pacstrap command to add more package/libraries to the runtime. `sudo pacstrap -C /home/$USER/.armv7h/etc/pacman.conf /home/$USER/.armv7h PACKAGES`


**NOTE: Only append this when running software that requires 32bit libraries, DO NOT EXPORT**

Add 32bit libraries to `LD_LIBRARY_PATH` :

```
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/arm-linux-gnueabihf/lib/:/home/$USER/.armv7h/usr/lib/
```

```
PATH=$PATH:/usr/arm-linux-gnueabihf/bin
```

**NOTE: Only append this when running software that requires 32bit libraries, DO NOT EXPORT**

---

### Setup Box86 & Box64

[Box64](https://github.com/ptitSeb/box64) is in the ArchLinuxCN repo. `sudo pacman -S box64-git`

or Manually compile PKGBUILD:

```
git clone https://github.com/ptitSeb/box64.git /home/$USER/alarm-multiarch-gaming/box64 && cd /home/$USER/alarm-multiarch-gaming/box64/pkgbuilds && cp -r ./PKGBUILD-arm64 ./PKGBUILD && makepkg --syncdeps -i
```

[Box86](https://github.com/ptitSeb/box86) need to manually compile PKGBUILD:

```
git clone https://github.com/ptitSeb/box86.git /home/$USER/alarm-multiarch-gaming/box86 && cd /home/$USER/alarm-multiarch-gaming/box86/pkgbuilds && cp -r ./PKGBUILD-arm64 ./PKGBUID && makepkg --syncdeps -i
```

After both are setup, run a test:

```
box64 --help
```

```
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/arm-linux-gnueabihf/lib/:/home/$USER/.armv7h/usr/lib box86 --help
```

**Box86 requires LD_LIBRARY_PATH to point to a 32bit libraries since Alarm doesn't have proper multiarch**

---

### Setup Steam (BROKEN)

Manually compile modifed Steam PKGBUILD:

```
cd /home/$USER/alarm-multiarch-gaming/steam-arm64 && makepkg --syncdeps -i
```

**After succesfull install:**

```
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/arm-linux-gnueabihf/lib:/home/$USER/.armv7h/usr/lib steam
```

---

**CREDITS COMING SOON**

