# ArchLinuxKVM
This is a guide how to install KVM and QEMU on Arch Linux

Please follow this steps carefully !

This install will skip some unneeded packages like virt-viewer libguestfs and vim

1. Check if you have a virtualization enabled

grep -Ec '(vmx|svm)' /proc/cpuinfo

If you recieve number bigger then 0 like 1 or higher you have virtualizaion enabled
If you recieve a 0 you must turn on virtualization settings in your BIOS/UEFI firmware

2. Make sure you Arch Linux is up to date

sudo pacman -Syyu

3. Install KVM and QEMU packages

sudo pacman -S qemu-full virt-manager dnsmasq bridge-utils vde2 openbsd-netcat ebtables

Download size should be around 136 Megabytes

If you get conflict warning say YES to remove package in conflict

4. Configure libvirtd

sudo nano /etc/libvirt/libvirtd.conf

Find (Control+F) and uncomment this two lines by removing POUND (#) character

#unix_sock_group = "libvirt"

#unix_sock_rw_perms = "0770"

Save modified file (Control+O) and exit nano (Control+X)

5. Add current user to libvirtd group

sudo usermod -aG libvirt $USER

sudo usermod -aG libvirt (username)

6. Start services

sudo systemctl enable libvirtd.service virtlockd.socket virtlogd.socket libvirtd.socket libvirtd-ro.socket libvirtd-admin.socket virtlockd-admin.socket virtlogd-admin.socket

sudo systemctl start libvirtd.service virtlockd.socket virtlogd.socket libvirtd.socket libvirtd-ro.socket libvirtd-admin.socket virtlockd-admin.socket virtlogd-admin.socket

8. start NAT network

List all networks 

sudo virsh net-list --all

Restart firewall

sudo firewall-cmd --reload

Start NAT network

sudo virsh net-start default 

make NAT start at startup 

sudo virsh net-autostart default
   
9. Done

Use virt-manager from terminal or Virtual Nachine Manager in app menu to use KVM
