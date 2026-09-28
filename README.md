# Fedora Post Installation Guide
Standard Fedora Post Installation Guide - Current Release Fedora 44
Things to do after installing Fedora 44

## RPM Fusion - Non-free Repositories
* Enable RPM Fusion for non-free repositories by installing:
```
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```
* Also, install app-stream metadata with:
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
* Add Complete Flatpak support by installing access to all Flathub repositories:
```
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```
* Disable now the unnecessary Fedora default Flathub repository:
```
flatpak remote-modify --disable fedora
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
* Wait approximately 5-10 minutes before rebooting as kernel modules get built. Then see if the module was built by checking version:
```
modinfo -F version nvidia
```
* Reboot:
```
sudo reboot
```

## Media Codecs
* Switch to FFMPEG for proper multimedia codecs by installing:
```
sudo dnf swap 'ffmpeg-free' 'ffmpeg' --allowerasing
```
* Update multimedia/GStreamer components while excluding currently broken broken packages:
```
sudo dnf update @multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin --exclude=libheif-freeworld --exclude=obs-studio-freeworld 
```
* Sound and video complementary packages:
```
sudo dnf group install -y sound-and-video
```

## OpenH264 for Firefox
* To access OpenH264 in Firefox install:
```
sudo dnf install -y openh264 gstreamer1-plugin-openh264 mozilla-openh264
```
* Enable OpenH264 in your system:
```
sudo dnf config-manager setopt fedora-cisco-openh264.enabled=1
```
* Then, enable OpenH264 in Firefox Settings

## Steam
