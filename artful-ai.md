<!-- SPDX-License-Identifier: zlib-acknowledgement -->
TODO: chatGPT in workflow
https://www.youtube.com/watch?v=sTeoEFzVNSc


sudo apt-get update
sudo apt-get -y upgrade
sudo apt-get -y install \
  gcc \
  make \
  pkg-config \
  apt-transport-https \
  ca-certificates

if ! [ -f /etc/modprobe.d/blacklist-nouveau.conf ]; then
  echo "nouveau is not blacklisted, doing so and rebooting"

  # Blacklist nouveau and rebuild kernel initramfs
  echo "blacklist nouveau
options nouveau modeset=0" >> blacklist-nouveau.conf
  sudo mv blacklist-nouveau.conf /etc/modprobe.d/blacklist-nouveau.conf
  sudo update-initramfs -u
  # NOTE: fter rebooting we need to run this file again
  sudo reboot
fi

# Check if nvidia driver is installed
if ! [ -f /usr/bin/nvidia-smi ]; then
  echo "nvidia driver is not installed, installing"
  # Install NVIDIA Linux toolkit 510.54
  wget https://us.download.nvidia.com/XFree86/Linux-x86_64/510.54/NVIDIA-Linux-x86_64-510.54.run
  chmod +x NVIDIA-Linux-x86_64-510.54.run
  sudo bash ./NVIDIA-Linux-x86_64-510.54.run
  rm NVIDIA-Linux-x86_64-510.54.run
fi

# Install CUDA toolkit 11.3 Upgrade 1 for Ubuntu 20.04 LTS
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/11.3.1/local_installers/cuda-repo-ubuntu2004-11-3-local_11.3.1-465.19.01-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu2004-11-3-local_11.3.1-465.19.01-1_amd64.deb
sudo apt-key add /var/cuda-repo-ubuntu2004-11-3-local/7fa2af80.pub
sudo apt-get update
sudo apt-get -y install cuda


ssh-add -L or simply look in ~/.ssh directory (this is essential for private key)

seems that packaged things in the cloud aren't all that flexible, e.g. ML-in-a-box cannot have independent components updated

important to run apt update on first running

have to use -O and enclose with "" for wget

annonyingly have to remove open source noveu driver
also install cuda not from apt repository

Based on what was trained 
(LAION 400M internet scraped image-text pairs, which contains violent and sexual images (as oppose to DALLE-2)), 
output may bias, e.g. nerd might bias towards wearing glasses 
