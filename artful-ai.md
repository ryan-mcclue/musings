<!-- SPDX-License-Identifier: zlib-acknowledgement -->
person image to 3d model: https://www.youtube.com/watch?v=6DOsWaxcHEM 
better auto-rigging: https://www.youtube.com/watch?v=XsAT3s8nrPE
better person prints: https://www.youtube.com/watch?v=xSkyLuRnt4g

can put ourselves into images, but have to train with ≈20GB VRAM using diffusers
20 photos (of various emotions) in 1:1 aspect ratio:
* 3 images full body 
* 5 naval upwards
* 12 face
set class to person
we save model weights, which can then be supplied to the webui?
in prompts use token and class, e.g. RYan person
low 'guidance scale' is more detailed?

HuggingFace hosts CompVis stable diffusion models? accept license agreement and use account access token

text to human animation: https://github.com/GuyTevet/motion-diffusion-model 

MonsterMash 2d-3d animation

HEGEMONY

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
Based on what was trained (LAION 400M internet scraped image-text pairs, which contains violent and sexual images (as oppose to DALLE-2)), output may bias, e.g. nerd might bias towards wearing glasses 

chatGPT

txt2img:
word:weight, word:weight (must weights to proceeding works as well to not override)
steampunk/low-poly 3D/minecraft rodent, intricate, pen and ink/canon m50, concept art, aerial
in reality, will have different prompt formats for particular, e.g. architecture
https://www.youtube.com/watch?v=oIAM3loi51s&t=184s

    "a cute magical flying dog, fantasy art, "
    "golden color, high quality, highly detailed, elegant, sharp focus, "
    "concept art, character concepts, digital painting, mystery, adventure",
    batch_size=3,

    A mysterious dark stranger visits the great pyramids of egypt, 
    high quality, highly detailed, elegant, sharp focus, 
    concept art, character concepts, digital painting,

"a pirate ship, clean line art, drawing, sketching, kitbash 3 d, art by hebron ppg"
https://lexica.art/?q=2ea3ae2a-4c8b-4d64-9c24-0924da5c759d


scale is how close to original?
batch/iterations is how many images to create? Or do we use a custom script? https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Custom-Scripts
seems that there are a variety of Stable Diffusion scripts to use, e.g. https://github.com/jquesnelle/txt2imghd 

img2img:
Maintaining likeness:
* Euler over Eular A
* Sampling steps removes noise, 15-30
* CFG scale 0.3-0.6
* Denoise strength 1
* lower CFG scale makes image more honest to original
--> is textual inversion just a variant of img2img keeping likeness?

interpolated videos:
timelapse between prompts

outpainting/inpainting:
dalle-2 much better here for photos

ChatGPT to generate commit messages: 
https://github.com/RomanHotsiy/commitgpt
