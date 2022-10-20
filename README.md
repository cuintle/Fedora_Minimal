<h1 align="center">Fedora Minimal</h1>
<br>
<br>
<p align="center">A <b>personal</b> minimal installation of Fedora operating system</p>
<br/>
<p align="center">⚠️⚠️⚠️ <b>BE CAREFUL</b> ⚠️⚠️⚠️</p>
<p align="center">This kickstart file reflects very much <b>my own personal taste</b>, so be aware of that and <b>use it at your own risk</b>.</p>  

> **Warning:**  
    - This is **NOT** a ready-to-use installation file. **It needs some editing to fit your needs**.  
    - This kickstart file is  intended to be used to install a **full flatpak system** (per-user flatpaks)  
    - **Do not** expect the resulting system to be a complete general purpose Fedora installation.  
    - This installation is **INTEL focused**, but you can uncomment **akmod-nvidia** in the **%packages** section to install NVidia driver.  

<br>

## Features   

 ### General  

- **FDE enabled** with LUKS2  
- **RPMFusion** repos enabled  
- **Testing repositories**  
- **Minimal GNOME Desktop Environment**  
  - GNOME Console  
  - GNOME Disks  
  - GNOME System Monitor  
  - GNOME Text Editor  
  - GNOME Tweaks  
  - Nautilus  
  - dconf Editor  
  - **Nothing more**  

 ### Partitioning  
  - **Full BTRFS**  
  - **BTRFS** subvolumes everywhere  
  - **SSD** optmizations  

 ### System Customizations  
  - **DNS** over TLS by default  
  - **DNF** optimizations  
  - **ZSH** as default user shell  
  - [**Micro**](https://micro-editor.github.io) as default terminal text editor
  - **IWD** instead of wpa_supplicant  
  - **Kernel Xanmod** copr enabled  
  - **Mesa git** copr enabled  
  - **Extra**  
    - [**Podman**](https://podman.io)
    - [**Httpie**](https://httpie.io)


## Usage  


The easiest way to use this installation file is by creating a 1MB **`EXT4`** partition labeled **`OEMDRV`** (**required**) on your USB stick and put the kickstart file there. The file must be named **`ks.cfg`** (**required**). Keep in mind that when you use this method the Anaconda installer will **dd** this partition, which means that the OEMDRV partition will no longer exists after the installation process.  

You can use [**Ventoy**](https://www.ventoy.net/en/index.html) to create a bootable USB stick preserving free space. On Ventoy, go to `"Option"` menu and then to `"Partition Configuration"` and then tick the `"Preserve some space at the end of the disk"` option.  

Now you can use GNOME Disks to create a 1MB partition labeled `OEMDRV` or you can run:  

```bash
$ sudo e2label /dev/xyz OEMDRV
```

## TEST  
Use **`virt-install`** to test the installation in a virtual machine.

```bash
$ qemu-img create -f qcow2 fedora_ks.qcow2 20G
```

```bash
$ virt-install --name Fedora_Minimal \
--memory 4092 --cpu host --vcpus 2 \
--location PATH_TO_ISO_FILE --disk fedora_ks.qcow2 \
--video virtio --virt-type kvm --boot uefi \
--transient --destroy-on-exit --osinfo detect=on,require=off \
--initrd-inject ks.cfg --extra-args="inst.ks=file:/ks.cfg console=tty0"

```

After installation process, run system installed on fedora_ks.qcow2 file.  

```bash
$ virt-install --name Fedora_Minimal \
--memory 4092 --cpu host --vcpus 2 --import \
--disk fedora_ks.qcow2 --video virtio \
--virt-type kvm --boot uefi --transient \
--destroy-on-exit --osinfo detect=on,require=off
```

## TESTED

|**Version**|Kickstart version|Bare Metal|Virtual Machine|Solution| Explanation|
|:----:|:----:|:----:|:-----:|:----:|:----:|
| ✔️**Fedora 37 Beta** | ✔️Minimal <br/>✔️Foss |  ✔️   |  ❌  |  install `gnome-session-xsession` package|  Only X11 session is working **on virtual-machine**, probably a DRM driver issue.|
| ✔️**Fedora 36** |  ✔️Minimal <br/>✔️Foss  |   ✔️    |  ✔️   |  N/A | N/A  |


## License  

This project is licensed under the terms of the MIT license.
