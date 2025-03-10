1. In your PC/Laptop/Server BIOS make sure the following is enabled in the BIOS:

Intel VT-d & VT-x – Intel Compatible list
All AMD CPUs from Bulldozer onwards should be compatible.

2. Get device IDs:
   lspci -nn

3. Enable IOMMU in GRUB (check Intel or AMD commands below - choose the right one)
   nano /etc/default/grub
   GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt"
   GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on"
   save and exit

4. run the command "update-grub"
   now reboot

5. Enable VFIO Modules
   nano /etc/modules
   Add the following modules:
   vfio
   vfio_iommu_type1
   vfio_pci
   vfio_virqfd
   Then, save and exit

Next run:
update-initramfs -u -k all
and reboot

7. GPU Isolation From the Host (amend the below to include the IDs of the device you want to isolate)
   echo "options vfio-pci ids=10de:1381,10de:0fbc disable_vga=1" > /etc/modprobe.d/vfio.conf

8. Blacklist GPU drivers (here are all that you would ever need)
   echo "blacklist radeon" >> /etc/modprobe.d/blacklist.conf
   echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf
   echo "blacklist nvidia" >> /etc/modprobe.d/blacklist.conf
   echo "blacklist nvidiafb" >> /etc/modprobe.d/blacklist.conf
   echo "blacklist nvidia_drm" >> /etc/modprobe.d/blacklist.conf

9. Create a new VM and add the GPU via hardware menu
   You may need to set it as primary GPU
   You may need to add a ROM BAR
