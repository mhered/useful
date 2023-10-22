# useful

Howto recover ubuntu when windows fucks up dual boot see https://askubuntu.com/a/1134107

power down 

plug a live USB of Ubuntu 20.04 

power up, press F2 to access UEFI

ensure USB is first in boot order

Select try Ubuntu. 

Connect to internet.

Open terminal and type

$ sudo add-apt-repository ppa:yannubuntu/boot-repair && sudo apt update
$ sudo apt-get install boot-repair && boot-repair

Select recommended fix 

This will fix grub and reinstall it and create a boot info log in a logbin.

Reboot and enjoy dualbooting.

Boot successfully repaired.

Please write on a paper the following URL:
https://paste.ubuntu.com/p/gwt2tF5m7p/

In case you still experience boot problem, indicate this URL to:
boot.repair@gmail.com or to your favorite support forum.

You can now reboot your computer.

Please do not forget to make your UEFI firmware boot on the Ubuntu 20.04.6 LTS entry (nvme0n1p1/efi/ubuntu/grubx64.efi file) !
If your computer reboots directly into Windows, try to change the boot order in your UEFI firmware.
If your UEFI firmware does not allow to change the boot order, change the default boot entry of the Windows bootloader.
For example you can boot into Windows, then type the following command in an admin command prompt:
bcdedit /set {bootmgr} path \EFI\ubuntu\grubx64.efi
