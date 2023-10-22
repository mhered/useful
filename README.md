# useful

## How to recover ubuntu when windows f**cks up dual boot.

Instructions based on: https://askubuntu.com/a/1134107

Symptoms: for no reason dual boot stops working.
- during reboot a message flashes:

```bash
Failed to open \EFI\BOOT\grubx64.efi - Not Found  
Failed to load image: Not Found    
start_image() returned Not Found, falling back to default order
Failed to open \EFI\BOOT\grubx64.efi - Not Found  
Failed to load image: Not Found    
start_image() returned Not Found,
```
- rebooting skips the grub menu that allows to choose OS and boots directly on Windows. 
Fix:

1. power down 

2. plug a live USB of Ubuntu 20.04 

3. Power up the laptop, access UEFI (in LG gram: press F2 during 3 secs)

4. Ensure USB is first in boot order

5. Select "Try Ubuntu" this will boot Ubuntu from USB

6. Configure Wifi to have internet connection.

7. Open a terminal and type:

```bash
$ sudo add-apt-repository ppa:yannubuntu/boot-repair && sudo apt update
$ sudo apt-get install boot-repair && boot-repair
```

8. Select "Recommended fix". This will fix grub and reinstall it and create a boot info log in a logbin.
The app provides a report in a pastebin to send to boot.repair@gmail.com if further troubleshooting is needed

10. Reboot
