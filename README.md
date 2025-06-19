# NVidia 550.163.01

Contains patches for compilation on Linux **6.15.2**-zen1-1-zen using gcc (GCC) **15.1.1** 20250425.

For Arch Linux only

## Source & Build
```bash
git clone https://github.com/xiadnoring/nvidia-550xx-dkms
cd nvidia-550xx-dkms
makepkg -si
```

## Install
```bash
pacman -U *.tar.zst
```

## Configuration

- Add ```nvidia-drm.modeset=1 nvidia-drm.fbdev=0``` to kernel params.

- Add some rules to ```/etc/modprobe.d/blacklist.conf```: 
```
blacklist nova_core
blacklist nova_drm
blacklist nouveau
```

- Make sure ```i915 nvidia nvidia_modeset nvidia_uvm nvidia_drm``` exists in MODULES=(...) ```/etc/mkinitcpio.conf```

- Make sure ```kms``` doesn't exist in HOOKS=(...) ```/etc/mkinitcpio.conf```

- Rebuild using shell command ```$ sudo mkinitcpio -P```

- Add ```nvidia nvidia-550xx-utils nvidia-550xx-dkms``` to ```IgnorePkg = ...``` in ```/etc/pacman.conf```

> [!CAUTION]
> Be careful to check the existence of linux kernel files in /boot
> Check dkms status using shell command: ```$ dkms status```

## Why
My computer doesn't work with the latest driver.
- DRM init freeze
- sddm freeze
- pc freeze
- all freezes

*I thought my nvidia was dead :(*

## Env (hyprland config)
```
## IF SYSTEMD
env = HYPRLAND_NO_SD_NOTIFY,1

## QT

env = QT_QPA_PLATFORM,wayland
env = QT_QPA_PLATFORM,wayland;xcb
env = QT_QPA_PLATFORMTHEME,qt5ct
env = QT_WAYLAND_DISABLE_WINDOWDECORATION,1
env = QT_QPA_PLATFORM,wayland;xcb
env = QT_AUTO_SCREEN_SCALE_FACTOR,1

## GTK

env = GDK_BACKEND,wayland,x11,*

# Backend

env = SDL_VIDEODRIVER,wayland
env = CLUTTER_BACKEND,wayland

# XDG

env = XDG_CURRENT_DESKTOP,Hyprland
env = XDG_SESSION_TYPE,wayland
env = XDG_SESSION_DESKTOP,Hyprland

## NVIDIA:

env = LIBVA_DRIVER_NAME,nvidia
env = GBM_BACKEND,nvidia-drm
env = __GLX_VENDOR_LIBRARY_NAME,nvidia
#env = __VK_LAYER_NV_optimus,NVIDIA_only 
env = __GLX_VENDOR_LIBRARY_NAME,nvidia
env = GBM_BACKENDS_PATH,/usr/lib/gbm
env = MOZ_WEBRENDER,1
env = MOZ_ENABLE_WAYLAND,1
env = __GL_VRR_ALLOWED, 0
env = XDG_MENU_PREFIX, arch-
```

## Packages
```bash
$ paru -Qs nvidia
local/egl-wayland 4:1.1.19-1
    EGLStream-based Wayland external platform
local/egl-x11 1.0.2-1
    NVIDIA XLib and XCB EGL Platform Library
local/libvdpau 1.5-3
    Nvidia VDPAU library
local/libxnvctrl 575.57.08-1
    NVIDIA NV-CONTROL X extension
local/nvidia-550xx-dkms 550.163.01-2
    NVIDIA drivers - module sources, 550 branch
local/nvidia-550xx-utils 550.163.01-2
    NVIDIA drivers utilities, 550 branch
local/nvidia-prime 1.0-5
    NVIDIA Prime Render Offload configuration and utilities
local/nvtop 3.2.0-1
    GPUs process monitoring for AMD, Intel and NVIDIA
local/opencl-nvidia-550xx 550.163.01-2
    OpenCL implemention for NVIDIA, 550 branch
```

```bash
$ paru -Qs egl
local/egl-wayland 4:1.1.19-1
    EGLStream-based Wayland external platform
local/egl-x11 1.0.2-1
    NVIDIA XLib and XCB EGL Platform Library
local/eglexternalplatform 1.2.1-1
    EGL External Platform interface
local/freeglut 3.6.0-2
    Free OpenGL Utility Toolkit
local/libglvnd 1.7.0-3
    The GL Vendor-Neutral Dispatch library
local/mesa-utils 9.0.0-7
    Essential Mesa utilities
local/nvidia-550xx-utils 550.163.01-2
    NVIDIA drivers utilities, 550 branch
local/wayland 1.23.1-2
    A computer display server protocol
```

[More here](https://github.com/xiadnoring/manapi-http/nvidia-550xx-dkms/blob/main/packages.list) | 
[archive.archlinux.org](https://archive.archlinux.org)

## Finish
```bash
$ nvidia-smi
Thu Jun 19 13:12:45 2025       
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 550.163.01             Driver Version: 550.163.01     CUDA Version: 12.4     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA GeForce RTX 4050 ...    Off |   00000000:01:00.0 Off |                  N/A |
| N/A   45C    P8              4W /   60W |      32MiB /   6141MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+
                                                                                         
+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|    0   N/A  N/A       677      G   /usr/lib/Xorg                                   4MiB |
|    0   N/A  N/A       820      G   Hyprland                                        1MiB |
|    0   N/A  N/A      5437    C+G   ...ble-wayland-ime --user-angle=vulkan          5MiB |
|    0   N/A  N/A     11724    C+G   nautilus                                        6MiB |
+-----------------------------------------------------------------------------------------+
```

## Thanks
Thanks to that good people
- [People from aur](https://aur.archlinux.org/packages/nvidia-550xx-utils)
- [People from cachyos](https://github.com/CachyOS/CachyOS-PKGBUILDS/tree/feat/nvidia-575/nvidia/nvidia-utils)