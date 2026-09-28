# Fedora Post Installation Guide
* Standard Fedora Post Installation Guide
* Version: Fedora 44
* Things to do after installing Fedora 44

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
* In case you have a legacy Nvidia GPU (Pascal, etc.), you must install a specific driver version with:
```
sudo dnf install akmod-nvidia-580xx
```
* In case you intent gaming you must install 32bit support with:
```
sudo dnf install
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
* If you intent gaming, install 32bit libraries with:
```
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

## Steam
