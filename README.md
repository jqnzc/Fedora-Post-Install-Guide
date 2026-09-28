# Fedora Linux Post Installation Guide (2026)
* Active: Fedora 44
* The following is a standard post-installation guide designed to help you setup a new Fedora installation, add RPM Fusion repositories for complete free and non-free software access, install Nvidia drivers and multimedia codecs, install Steam for gaming, etc.

## Things to do after installing Fedora 44

## DNF Optimization
* Edit your dnf configuration with `sudo nano /etc/dnf/dnf.conf`
* Enhance dnf with the parameters you see fit:
```
max_parallel_downloads=10 # For increasing installation speed (Recommended)
defaultyes=True # For auto-confirming prompt
keepcache=True # For avoiding redownload of packages
```

## RPM Fusion
* Fedora disables many free and non-free .rpm packages by default. To use software like Steam, Discord, multimedia codecs, etc. it is advised to enable RPM Fusion with:
```
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```
* Install app-stream metadata with:
```
sudo dnf install -y rpmfusion-free-appstream-data rpmfusion-nonfree-appstream-data
```
* Update your system with `sudo dnf update -y`
* Reboot with `sudo reboot`

## Firmware
* If your system supports firmware update delivery through LVFS, update your device firmware with:
```
fwupdmgr refresh --force
```
* List devices with available updates with:
```
fwupdmgr get-devices
```
* Fetches list of available updates with:
```
fwupdmgr get-updates
```
* Update with:
```
fwupdmgr update
```

## Flatpak
* Fedora doesn't include all non-free flatpaks by default. In-case you forgot to check the "Enable Third Party Repositories" option on initial boot, the command below enables access to all the flathub flatpaks
* Add complete Flatpak support by installing access to all flathub repositories with:
```
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```
* Disable the now unnecessary default Fedora flathub repository with:
```
flatpak remote-modify --disable fedora
```

## Legacy NVIDIA Drivers
* Update your system with `sudo dnf update -y`
* In case you have a legacy Nvidia GPU (Pascal, Maxwell, Volta, etc.) install 580xx series with:
```
sudo dnf install xorg-x11-drv-nvidia-580xx akmod-nvidia-580xx
```
* Add CUDA 580xx support (sometimes needed for nvidia-smi to work) with:
```
sudo dnf install xorg-x11-drv-nvidia-580xx-cuda
```
* Wait approximately 5-10 minutes before rebooting as kernel modules get built. Then see if the module was built by checking version with:
```
modinfo -F version nvidia
```
* Reboot with `sudo reboot`

## NVIDIA Drivers
* Update your system with `sudo dnf update -y`
* Install Nvidia drivers with:
```
sudo dnf install xorg-x11-drv-nvidia akmod-nvidia
```
* Install CUDA support (for AI, graphics rendering, game development, etc.) with:
```
sudo dnf install xorg-x11-drv-nvidia-cuda
```
* Wait approximately 5-10 minutes before rebooting as kernel modules get built. Then see if the module was built by checking version with:
```
modinfo -F version nvidia
```
* Reboot with `sudo reboot`

## Media Codecs
* Switch to FFMPEG for proper multimedia codecs with:
```
sudo dnf swap 'ffmpeg-free' 'ffmpeg' --allowerasing
```
* Update multimedia/GStreamer components with:
```
sudo dnf update @multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin --exclude=libheif-freeworld --exclude=obs-studio-freeworld 
```
* Add complementary sound and video packages with:
```
sudo dnf group install -y sound-and-video
```
* Add hardware acceleration VA-API video decoding with:
```
sudo dnf install ffmpeg-libs libva libva-utils
```

## Intel Codecs
* Install Intel multimedia codecs with:
```
sudo dnf swap libva-intel-media-driver intel-media-driver --allowerasing
```
* Then:
```
sudo dnf install libva-intel-driver
```

## OpenH264 for Firefox
* Access OpenH264 in Firefox with:
```
sudo dnf install -y openh264 gstreamer1-plugin-openh264 mozilla-openh264
```
* Enable OpenH264 in your system with:
```
sudo dnf config-manager setopt fedora-cisco-openh264.enabled=1
```
* Then, enable OpenH264 in Firefox Settings

## VSCodium
* Add the repositories with:
```
sudo tee -a /etc/yum.repos.d/vscodium.repo << 'EOF'
[gitlab.com_paulcarroty_vscodium_repo]
name=gitlab.com_paulcarroty_vscodium_repo
baseurl=https://paulcarroty.gitlab.io/vscodium-deb-rpm-repo/rpms/
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/raw/master/pub.gpg
metadata_expire=1h
EOF
```
* Install codium with:
```
sudo dnf install codium
```
* As found in https://vscodium.com/

## Steam
* Install Steam with:
```
sudo dnf install steam
```
* 32bit support for legacy Nvidia 580xx drivers:
```
sudo dnf install xorg-x11-drv-nvidia-580xx-libs.i686 xorg-x11-drv-nvidia-580xx-cuda-libs.i686
```
* 32bit support for Nvidia drivers:
```
sudo dnf install xorg-x11-drv-nvidia-libs.i686 xorg-x11-drv-nvidia-cuda-libs.i686
``` 
* Recommended environment variables for proper Nvidia GPU usage:
```
__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __VK_LAYER_NV_optimus=NVIDIA_only __GL_THREADED_OPTIMIZATIONS=1 %command%
```
* Optional gaming related packages for CPU and GPU control:
```
sudo dnf install mangohud goverlay mangohud.i686 goverlay.i686
```

## References
* Fedora 44 Post Install Guide (https://github.com/devangshekhawat/Fedora-44-Post-Install-Guide)
* Fedora 44 Post Install Guide (https://techhut.tv/fedora-44-post-install-guide)
* Fedora 44 KDE Setup (https://github.com/26zl/fedora-kde-setup)
* Nvidia on Fedora Desktops (https://github.com/fady-saied/Nvidia-Fedora-Guide)
* Nvidia Optional Components (https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/optional-components.html)
