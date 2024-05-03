# Arch Linux Arm Multiarch Gaming

## Experimental

This is an experimental setup for armhf multiarch support on [Arch Linux Arm](https://archlinuxarm.org/) (alarm) using the [ArchLinuxCN](https://github.com/archlinuxcn/repo) repo. Only basic programs and a few games run currently. This method still uses a 32bit environment, but only as a runtime folder for 32bit libraries since alarm isn't originally setup for multiarch. Some PKGBUILDS are provided for easy of use, others are pulled from their repo. If you have and insight, suggestions, or scripts to make this better, please share. 

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
sudo pacstrap -C ./etc/pacman.conf /opt/armv7h base base-devel chromium pipewire pipewire-alsa pipewire-pulse
```

Use this pacstrap command to add more package/libraries to the runtime when needed later on. `sudo pacstrap -C /opt/armv7h/etc/pacman.conf /opt/armv7h PACKAGES`

Use can also use `sudo arch-chroot /opt/armv7h/` to chroot into the runtime and manage for there with `pacman`

---

**NOTE: Only append this when running software that requires 32bit libraries, DO NOT EXPORT**

Add MultiArch to `/etc/ld.so.conf.d/` :

```
sudo cp -r /home/$USER/alarm-multiarch-gaming/etc/ld.so.conf.d/* /etc/ld.so.conf.d/ && sudo ldconfig
```

Optional, add `/usr/arm-linux-gnueabihf/bin` to `PATH` :

```
export PATH=$PATH:/usr/arm-linux-gnueabihf/bin
```

For programs that require 32bit libgl mesa, we need to add 32bit driver to `LIBGL_DRIVERS_PATH` :

```
export LIBGL_DRIVERS_PATH=/usr/lib/dri/:/opt/dri/armv7h/usr/lib/dri/
```
This can be added to `~/.bashrc` or `.bash_profile` for ease of use.

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
box86 --help
```

---

### Setup Steam (BROKEN)

Manually compile modifed Steam PKGBUILD:

```
cd /home/$USER/alarm-multiarch-gaming/steam-arm64 && makepkg --syncdeps -i
```

**After succesfull install:**

```
steam
```

---

**CREDITS COMING SOON**

