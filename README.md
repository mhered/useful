# useful
1. [Recover after Windows corrupts dual boot](#recover-after-windows-corrupts-dual-boot)
2. [Download video with subtitles to watch offline](#Download-video-with-subtitles-to-watch-offline)
3. [Setup git in new computer](#Setup-git-in-new-computer)

   
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

### Fix

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
