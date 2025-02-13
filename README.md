# Debian-on-Avell-Storm-470-Black
tutorial to install and setting Debian on your Avell Sotrm 470 Black
- https://avell.com.br/storm-470-black
- After a fresh installation from Debian 12.9

# SourcesList

- Adding components and commenting out the deb-src lines.
    
    ```bash
    # deb cdrom:[Debian GNU/Linux 12.9.0 _Bookworm_ - Official amd64 NETINST with firmware 20250111-10:54]/ bookworm contrib main non-free-firmware
    
    deb http://deb.debian.org/debian/ bookworm main non-free-firmware contrib non-free
    # deb-src http://deb.debian.org/debian/ bookworm main non-free-firmware
    
    deb http://security.debian.org/debian-security bookworm-security main non-free-firmware contrib non-free
    # deb-src http://security.debian.org/debian-security bookworm-security main non-free-firmware
    
    # bookworm-updates, to get updates before a point release is made;
    # see https://www.debian.org/doc/manuals/debian-reference/ch02.en.html#_updates_and_backports
    deb http://deb.debian.org/debian/ bookworm-updates main non-free-firmware contrib non-free
    # deb-src http://deb.debian.org/debian/ bookworm-updates main non-free-firmware
    
    # This system was installed using small removable media
    # (e.g. netinst, live or single CD). The matching "deb cdrom"
    # entries were disabled at the end of the installation process.
    # For information about how to configure apt package sources,
    # see the sources.list(5) manual
    ```
- Upgrade
    
    ```bash
    sudo apt upgrade
    ```

# Graphic Cards

- Nvidia drivers and Intel UHD graphics
    
    ```bash
    sudo apt install nvidia-driver firmware-misc-nonfree
    ```
    

# Network Interface Card

- Note: It is expected that the YT6801 drivers will be included in future versions of the Linux kernel.
    - https://github.com/silent-reader-cn/yt6801
    - https://github.com/bartvdbraak/yt6801
    - For lazy people.
        - https://deb.tuxedocomputers.com/ubuntu/pool/main/t/tuxedo-yt6801/tuxedo-yt6801_1.0.29tux0_all.deb
        - https://github.com/dante1613/Motorcomm-YT6801/raw/main/tuxedo-yt6801/tuxedo-yt6801_1.0.28-1_all.deb
        - https://github.com/dante1613/Motorcomm-YT6801/blob/f514f089a0b239bac8cfbf37bad521d00d281278/Debian%20-%20instruction.md

# Keyboard settings

- Avell unofficial Control Center
    - Credits: https://github.com/rodgomesc
    - Credits: https://github.com/EnricoMi
    - Installation
        
        ```bash
        # If you don't have pipx just install with 
        # $ apt install pipx
        sudo pipx install avell-unofficial-control-center
        
        sudo pipx ensurepath
        ```
        
    - Merge Pull Request 76
        
        ```bash
        ph-avell:~/Downloads$ git clone https://github.com/rodgomesc/avell-unofficial-control-center.git
        Cloning into 'avell-unofficial-control-center'...
        remote: Enumerating objects: 218, done.
        remote: Counting objects: 100% (39/39), done.
        remote: Compressing objects: 100% (8/8), done.
        remote: Total 218 (delta 34), reused 31 (delta 31), pack-reused 179 (from 1)
        Receiving objects: 100% (218/218), 54.93 KiB | 242.00 KiB/s, done.
        Resolving deltas: 100% (118/118), done.
        
        ph-avell:~/Downloads$ cd avell-unofficial-control-center/
        
        ph-avell:~/Downloads/avell-unofficial-control-center$ git remote -v 
        origin	https://github.com/rodgomesc/avell-unofficial-control-center.git (fetch)
        origin	https://github.com/rodgomesc/avell-unofficial-control-center.git (push)
        
        ph-avell:~/Downloads/avell-unofficial-control-center$ git remote add rodgomesc https://github.com/rodgomesc/avell-unofficial-control-center.git
        
        ph-avell:~/Downloads/avell-unofficial-control-center$ git fetch rodgomesc pull/76/head:pr-76
        remote: Enumerating objects: 22, done.
        remote: Counting objects: 100% (22/22), done.
        remote: Compressing objects: 100% (11/11), done.
        remote: Total 22 (delta 14), reused 19 (delta 11), pack-reused 0 (from 0)
        Unpacking objects: 100% (22/22), 6.95 KiB | 3.48 MiB/s, done.
        From https://github.com/rodgomesc/avell-unofficial-control-center
         * [new ref]         refs/pull/76/head -> pr-76
        
        ph-avell:~/Downloads/avell-unofficial-control-center$ git merge pr-76
        Updating e32e243..4f6bdde
        Fast-forward
         aucc/core/handler.py |  20 ++++----------------
         aucc/main.py         | 167 +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++------------------------------------------------
         2 files changed, 121 insertions(+), 66 deletions(-)
        ```
        
    - Copy modified files
        
        ```bash
        ph-avell:~/Downloads/avell-unofficial-control-center$ cp -v aucc/core/handler.py ~/.local/pipx/venvs/avell-unofficial-control-center/lib/python3.11/site-packages/aucc/core/handler.py
        
        ph-avell:~/Downloads/avell-unofficial-control-center$ cp -v aucc/main.py ~/.local/pipx/venvs/avell-unofficial-control-center/lib/python3.11/site-packages/aucc/main.py
        ```
        
- Keyboard LEDs
    
    ```bash
    ph-avell:~$ aucc -l
    [1] device #1 vendor=1165, product=24587
    [2] device #2 vendor=1165, product=28673
    
    ph-avell:~$ aucc --help
    usage: aucc [-h] (-l | -c COLOR | -H COLOR COLOR | -V COLOR COLOR | -s STYLE | -d) [-v VENDOR] [-p PRODUCT] [-D DEVICE] [-b {1,2,3,4}] [--speed {1,2,3,4,5,6,7,8,9,10}]
    
    Colors available:
    [red|green|blue|teal|pink|purple|white|yellow|orange|olive|maroon|brown|gray|skyblue|navy|crimson|darkgreen|lightgreen|gold|violet] 
    
    options:
      -h, --help            show this help message and exit
      -l, --list-devices    List all available devices. Use --vendor or --product to look for other vendors.
      -c COLOR, --color COLOR
                            Select a single color for all keys.
      -H COLOR COLOR, --h-alt COLOR COLOR
                            Horizontal alternating colors
      -V COLOR COLOR, --v-alt COLOR COLOR
                            Vertical alternating colors
      -s STYLE, --style STYLE
                            One of (rainbow, marquee, wave, raindrop, aurora, random, reactive, breathing, ripple, reactiveripple, reactiveaurora, fireworks). Additional single colors are
                            available for the following styles: raindrop, aurora, random, reactive, breathing, ripple, reactiveripple, reactiveaurora and fireworks. These colors are: Red (r),
                            Orange (o), Yellow (y), Green (g), Blue (b), Teal (t), Purple (p). Append those styles with the start letter of the color you would like (e.g. rippler = Ripple Red
      -d, --disable         Turn keyboard backlight off
      -v VENDOR, --vendor VENDOR
                            Set vendor id (e.g. 1165 or 0x048d).
      -p PRODUCT, --product PRODUCT
                            Set product id.
      -D DEVICE, --device DEVICE
                            Select device (1, 2, ...). Use -l to list available devices.
      -b {1,2,3,4}, --brightness {1,2,3,4}
                            Set brightness, 1 is minimum, 4 is maximum.
      --speed {1,2,3,4,5,6,7,8,9,10}
                            Set style speed. 1 is fastest. 10 is slowest
    
    # Set Keyboard LEDs Green 
    ph-avell:~$ aucc -D 1 -c green
    
    # Set Keyboard LEDs Blue 
    ph-avell:~$ aucc -D 1 -c blue
    
    # Disable Backlight LED
    ph-avell:~$ aucc -D 2 -d
    ```
    
- Creating a service
    - Symbolic link
        
        ```bash
        ph-avell:~$ sudo ln -s ~/.local/bin/aucc /usr/local/bin/aucc
        ```
        
    - aucc.service file
        
        ```bash
        ph-avell:~$ cat /etc/systemd/system/aucc.service 
        [Unit]
        Description=Avell Unofficial Control Center
        After=network.target
        
        [Service]
        ExecStart=/usr/local/bin/aucc -D 1 -c green
        Restart=always
        User=root
        Group=root
        StandardOutput=append:/var/log/aucc_out.log
        StandardError=append:/var/log/aucc_err.log
        
        [Install]
        WantedBy=multi-user.target
        ```
        
- Enabling
    
    ```bash
    ph-avell:~$ sudo systemctl enable aucc.service
    ```
