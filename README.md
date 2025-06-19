# useful
1. [Recover after Windows corrupts dual boot](#recover-after-windows-corrupts-dual-boot)
   
2. [Download video with subtitles to watch offline](#download-video-with-subtitles-to-watch-offline)
   
4. [Setup git in new computer](#setup-git-in-new-computer)
   
4. [Open SLDPRT CAD without SolidWorks](#open-sldprt-cad-without-solidworks)
   
5. [Embed video in Markdown](#embed-video-in-markdown)

5. [Recover large messy change in code](#recover-large-messy-change-in-code)


## Recover after Windows corrupts dual boot

### Problem
For no apparent reason dual boot stops working.
- the following message flashes briefly during reboot:

```bash
Failed to open \EFI\ubuntu\grubx64.efi - Not Found  
Failed to load image: Not Found    
start_image() returned Not Found, falling back to default loader
Failed to open \EFI\ubuntu\grubx64.efi - Not Found  
Failed to load image: Not Found    
start_image() returned Not Found
```
- the grub menu that allows choosing OS is skipped, and instead Windows boots directly.

### Fix 1

Instructions based on: https://askubuntu.com/a/1134107

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

### Fix 2

May need to disable fast boot and early threat checks for above to work

There are 4 ways to enable/disable fast boot, see:  https://www.youtube.com/watch?v=M3nbEqSBs04

1) control panel > hardware and sound > power options > behaviour closing lid> change settings unavailable > untick fast startup

2. terminal > right-click>run as admin > 

```shell
> powercfg /h off
```
3 disable in group policy editor 
but, need to install `gpedit` first, didn't succeed 
create a file `gpedit-install.bat`:
```
FOR %F IN ("%SystemRoot%\servicing\Packages\Microsoft-Windows-GroupPolicy-ClientTools-Package~*.mum") DO (DISM /Online /NoRestart /Add-Package:"%F")
FOR %F IN ("%SystemRoot%\servicing\Packages\Microsoft-Windows-GroupPolicy-ClientExtensions-Package~*.mum") DO (DISM /Online /NoRestart /Add-Package:"%F")
```
execute it as admin
```
> gpedit.msc
```

### Fix 3

If previous ones do not work, try manually
Source: https://askubuntu.com/questions/88384/how-can-i-repair-grub-how-to-get-ubuntu-back-after-installing-windows/88432
(CHECK SOURCE THIS IS NOT COMPLETE)

```
sudo fdisk -l # 1. Determine the partition number of your main partition and EFI
sudo mount /dev/nvme0n1p7 /mnt  # 2. mount the main Linux sistem
for i in /sys /proc /run /dev; do sudo mount --rbind "$i" "/mnt$i"; done # 3. some needed bindings (?)
sudo mount /dev/nvme0n1p1 /mnt/boot/efi # 4. mount EFI
sudo chroot /mnt # 5. update grub
update-grub
grub-install /dev/sda # 6. just, in case reinstall grub
update-grub
exit
reboot
```
## Download video with subtitles to watch offline

### Problem
You are boarding a plane and need to quickly download a video to finish watching it during the flight. And you want subtitles.

### Fix

1. If needed, install `yt-dlp`

```bash
$ sudo curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp
$ sudo chmod a+rx /usr/local/bin/yt-dlp  # Make executable
```
2. Type in terminal the command below replacing the video code e.g. `uf4zOigzTFo`. This downloads video and subtitles as 2 separate files e.g. `.vtt` and `.webp` to the current folder.

```bash
$ yt-dlp --write-sub --write-auto-sub --sub-lang "en.*" https://www.youtube.com/watch?v=uf4zOigzTFo
```

3. Play the video in VLC. In Subtitles select the appropriate `vtt` file. Note for some reason subtitles work in VLC but not in the default ubuntu player

## Setup git in new computer

### Problem 
You try to git push and git asks for user/pwd, 

### Fix

1. go to github, [create a Personal Access Token PAT (classic)](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic)

2. Add your user info, set up credential manager to store the PAT next time:
```bash
$ git config --global user.email "my email"
$ git config --global user.name "my name"
$ git config --global credential.helper store # to store PAT in the credential manager
```
3. Next time you try to push, git requests username/PAT one last time and the credential manager stores it:
```bash
$ git push
Username for 'https://github.com': # enter your username
Password for 'https://mhered@github.com': # enter your PAT
```

## Open SLDPRT CAD without SolidWorks

### Problem
many CAD files available for download are in SLDPRT format and SolidWorks requires suscription

### Fix
* Create account in [OnShape](https://cad.onshape.com/) (with spam user), open file then export as STL or STEP
* [AnyConv](https://anyconv.com/) but rather unreliable: scale not respected, or failure to convert 
* [ConvertCADFiles](https://convertcadfiles.com/): one free trial, afterwards pay per conversion
* Not tested: [download eDrawing](https://www.solidworks.com/support/free-downloads) (for Windows)

## Embed video in Markdown

### Problem
Markdown does not display mp4 and converting directly to gif yields huge files

### Fix
1. record live video with iphone (optionally edit it in the phone using imovie) / save desktop screencast with kazaam to mp4 
2. edit mp4 in shotcut: cut to length, speed up?, select loop, export as gif animation
3. upload gif to https://www.iloveimg.com/, resize by percentage 75% smaller

## Recover large messy change in code

### Problem
You made too many changes in your code without commiting, it does not work and you cannot debug. You want to go back and reapply changes step by step

### Fix

```bash
# 1. stash
git stash push -u -m "Full backup before debugging"

# 2. go back to last safe commit
git reset --hard HEAD

# 3. create a patch file
git stash show -p stash@{0} > changes.patch

# 4. Create a new branch to work on
git checkout -b debug-patch

# 5. Apply the full patch (it becomes unstaged changes)
git apply changes.patch

# 6. Use Git's interactive staging to pick parts
git add -p
git commit -m "First clean chunk of change"

# Repeat until all desired changes are committed

# Abort if needed
git reset --hard

# when done: 
# drop stash
git stash drop stash@{0}

# merge the branch and delete it
git checkout main
git merge debug-patch
git branch -d debug-patch


```



| Step             | Command                               |
| ---------------- | ------------------------------------- |
| Apply full patch | `git apply changes.patch`             |
| Stage part of it | `git add -p`                          |
| Commit           | `git commit -m "..."`                 |
| Repeat           | `git add -p`, then `git commit` again |
| Abort if needed  | `git reset --hard`                    |
