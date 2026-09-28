# Fedora Post Installation Guide
* Standard Fedora Post Installation Guide
* Version: Fedora 44
* Things to do after installing Fedora 44

## DNF Configuration
* Edit your dnf configuration with:
```
sudo nano /etc/dnf/dnf.conf
```
* Enhance download speed with:
```
max_parallel_downloads=10
```

## RPM Fusion - Non-free Repositories
* Fedora has disabled the repositories for a lot of free and non-free .rpm packages by default. Follow this if you want to use non-free software like Steam, Discord, multimedia codecs, etc. As a general rule of thumb it is advised to do this to get access to many mainstream useful programs
* Enable RPM Fusion for third party repositories with:
```
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```
* Install app-stream metadata with:
```
sudo dnf install -y rpmfusion-free-appstream-data rpmfusion-nonfree-appstream-data
```
* Update your system with:
```
sudo dnf update -y
```
* Reboot:
```
sudo reboot
```

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
* Update your system:
```
sudo dnf update -y
```
* In case you have a legacy Nvidia GPU (Pascal, Maxwell, Volta, etc.) install 580xx series with:
```
sudo dnf install xorg-x11-drv-nvidia-580xx akmod-nvidia-580xx
```
* Add CUDA 580xx support (sometimes needed for nvidia-smi to work) with:
```
sudo dnf install xorg-x11-drv-nvidia-580xx-cuda
```

## NVIDIA Drivers
* Update your system:
```
sudo dnf update -y
```
* Install Nvidia drivers with:
```
sudo dnf install akmod-nvidia
```
* Install CUDA support (for AI, graphics rendering, game development, etc.) with:
```
sudo dnf install xorg-x11-drv-nvidia-cuda
```
* Wait approximately 5-10 minutes before rebooting as kernel modules get built. Then see if the module was built by checking version with:
```
modinfo -F version nvidia
```
* Reboot:
```
sudo reboot
```

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
