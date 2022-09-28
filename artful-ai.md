<!-- SPDX-License-Identifier: zlib-acknowledgement -->
person image to 3d model: https://www.youtube.com/watch?v=6DOsWaxcHEM 

OhMyPrints WallApp, canvas photo prints

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

is command line a lot faster than gradio (gradio restricts VRAM usage?)

seems that packaged things in the cloud aren't all that flexible, e.g. ML-in-a-box cannot have independent components updated

important to run apt update on first running

have to use -O and enclose with "" for wget

annonyingly have to remove open source noveu driver
also install cuda not from apt repository

as python concept of CWD is strange, must run from outside directory

--------
Based on what was trained (LAION 400M internet scraped image-text pairs), output may bias, e.g. nerd might bias towards wearing glasses 

txt2img:
word:weight, word:weight (must weights to proceeding works as well to not override)
steampunk/low-poly 3D/minecraft rodent, intricate, pen and ink/canon m50, concept art, aerial
in reality, will have different prompt formats for particular, e.g. architecture
https://www.youtube.com/watch?v=oIAM3loi51s&t=184s

scale is how close to original?
batch/iterations is how many images to create? Or do we use a custom script? https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Custom-Scripts
seems that there are a variety of Stable Diffusion scripts to use, e.g. https://github.com/jquesnelle/txt2imghd 

img2img:
Euler, lower CFG scale makes image more honest to original
--> is textual inversion just a variant of img2img keeping likeness?

interpolated videos:
timelapse between prompts

outpainting:


inpainting:
generate mask by going into gimp and covering areas to be removed with white and then save that image as a mask
