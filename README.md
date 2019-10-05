# Battle.net install guide
Install Dependencies (Debian)
```
sudo apt install -y libc6 libgcc1 libglib2.0-0 libgphoto2-6 libgphoto2-port12 libgstreamer-plugins-base1.0-0 libgstreamer1.0-0 liblcms2-2 libldap-2.4-2 libmpg123-0 libopenal1 libpulse0 libudev1 libx11-6 libxext6 libxml2 zlib1g libasound2-plugins libncurses5 libcapi20-3 libcups2 libdbus-1-3 libfontconfig1 libfreetype6 libglu1-mesa libgnutls30 libgsm1 libgssapi-krb5-2 libgtk-3-0 libkrb5-3 libncurses5 libodbc1 libosmesa6 libpcap0.8 libpng16-16 libsane1 libsdl2-2.0-0 libtiff5 libv4l-0 libva-drm2 libva-x11-2 libva2 libvulkan1 libxcomposite1 libxcursor1 libxfixes3 libxi6 libxinerama1 libxrandr2 libxrender1 libxslt1.1 libxxf86vm1 gnutls-bin cabextract
sudo apt install -y libgnutls30:i386 libldap-2.4-2:i386 libgpg-error0:i386 libxml2:i386 libasound2-plugins:i386 libsdl2-2.0-0:i386 libfreetype6:i386 libdbus-1-3:i386 libsqlite3-0:i386
```

Install Dependencies (Arch Linux)
```
sudo pacman -Sy giflib lib32-giflib libpng lib32-libpng libldap lib32-libldap gnutls lib32-gnutls mpg123 lib32-mpg123 openal lib32-openal v4l-utils lib32-v4l-utils libpulse lib32-libpulse libgpg-error lib32-libgpg-error alsa-plugins lib32-alsa-plugins alsa-lib lib32-alsa-lib libjpeg-turbo lib32-libjpeg-turbo sqlite lib32-sqlite libxcomposite lib32-libxcomposite libxinerama lib32-libgcrypt libgcrypt lib32-libxinerama ncurses lib32-ncurses opencl-icd-loader lib32-opencl-icd-loader libxslt lib32-libxslt libva lib32-libva gtk3 lib32-gtk3 gst-plugins-base-libs lib32-gst-plugins-base-libs vulkan-icd-loader lib32-vulkan-icd-loader cups samba alsa alsa-utils alsa-tools gnutls libpng wine-mono lib32-libxml2 lib32-mpg123 lib32-lcms2 lib32-giflib lib32-libpng lib32-gnutls pygtk python2-dbus lib32-libpulse lib32-fontconfig lib32-libxcomposite lib32-libxrender  lib32-libxslt lib32-gnutls lib32-libxi lib32-libxrandr lib32-libxinerama lib32-libcups lib32-freetype2 lib32-libpng lib32-openal python-pyopencl lib32-v4l-utils lib32-libxcursor lib32-mpg123 lib32-sdl xf86-video-intel lib32-mesa-libgl nss-mdns --needed --noconfirm
```
Build my custom WINE build with the low latency audio patch already applied.

The guide on building my custom wine build with my audio patch integrated can be found here [Custom WINE build + low latency audio patch](https://github.com/BeanGreen247/Custom-WINE-build-low-latency-audio-patch).

With that please reboot and proceed with the guide

## The guide

Create the main directory (we will be using this directory for all the configurations and tweaks....may be different in your case)
```
sudo mkdir /mnt/84C2FF4EC2FF42CA/battlenet-wine-prefix
```
Create wine prefix
```
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/battlenet-wine-prefix WINEARCH=win64 winecfg
```
Install winetricks
```
wget https://raw.githubusercontent.com/Winetricks/winetricks/master/src/winetricks
chmod +x winetricks
sudo mv -v winetricks /usr/local/bin
```
Install .NET Framework
```
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/battlenet-wine-prefix WINEARCH=win64 winetricks -q --force dotnet48 vcrun2003 vcrun2005 vcrun2008 vcrun2012 vcrun2013 directplay 
```
Lower audio latency
```
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/battlenet-wine-prefix WINEARCH=win64 winetricks sound=alsa
cat > dsound.reg << "EOF"
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Wine\DirectSound]
"HelBuflen"="128"
"SndQueueMax"="1"
EOF
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/battlenet-wine-prefix WINEARCH=win64 wine regedit dsound.reg
```
Download Battlenet Launcher

https://www.battle.net/download/getInstallerForGame?os=win&locale=enUS&version=LIVE&gameProgram=BATTLENET_APP

Install dxvk 1.4.2

Download https://github.com/doitsujin/dxvk/releases/download/v1.4.2/dxvk-1.4.2.tar.gz

Extract dxvk-1.4.2.tar.gz and copy its contents
```
sudo cp -r dxvk-1.4.2/x64/* /mnt/84C2FF4EC2FF42CA/battlenet-wine-prefix/drive_c/windows/system32/
sudo cp -r dxvk-1.4.2/x32/* /mnt/84C2FF4EC2FF42CA/battlenet-wine-prefix/drive_c/windows/system/
```
Open winecfg
```
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/battlenet-wine-prefix/ WINEARCH=win64 winecfg
- Set Windows version to Windows 7 if not done already
- Go to libraries and add
>>d3d10
>>d3d10_1
>>d3d10core
>>d3d11
>>dxgi
- Set them to native and click apply
- Next go to graphics tick "Allow the window manager to control the windows"
- Next go to Staging and tick the first four
```
Next install DirectX

To do so download the DirectX web installer
https://www.microsoft.com/en-us/download/details.aspx?id=8109

Then launch it using the following command
```
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/battlenet-wine-prefix/ WINEARCH=win64 wine ~/Downloads/directx_Jun2010_redist.exe
```
It will aske where to install. Pick C:\

Once done launch the installer
```
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/battlenet-wine-prefix/ WINEARCH=win64 wine /mnt/84C2FF4EC2FF42CA/battlenet-wine-prefix/drive_c/DXSETUP.exe
```

Once finished with the install continue.

Install the Battlenet Launcher
```
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/battlenet-wine-prefix/ WINEARCH=win64 wine ~/Downloads/Battle.net-Setup.exe
```
After installing please continue

Create a custom terminal command for launching Battlenet Launcher!
```
cat > battlenet << "EOF"
#!/bin/sh
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/battlenet-wine-prefix/ WINEARCH=win64 wine /mnt/84C2FF4EC2FF42CA/battlenet-wine-prefix/drive_c/Program\ Files\ \(x86\)/Battle.net/Battle.net\ Launcher.exe

EOF
```
Create a custom terminal command for killing Battlenet Launcher!
```
cat > battlenetkill << "EOF"
#!/bin/sh
sudo WINEPREFIX=/mnt/84C2FF4EC2FF42CA/battlenet-wine-prefix/ WINEARCH=win64 wineserver -k
sudo killall -9 /opt/wine-staging/bin/*
EOF
```
Make the commands executable and make them accesable from the terminal
```
sudo mv battlenet /usr/bin/
sudo chmod +x /usr/bin/battlenet
sudo mv battlenetkill /usr/bin/
sudo chmod +x /usr/bin/battlenetkill
```
## Troubleshooting
If you get any error related to Windows, dlls missing or exes missing, just remove the prefix folder and start again
```
sudo rm -rf /mnt/84C2FF4EC2FF42CA/battlenet-wine-prefix
```
If the game refuses to run make sure to test if X11 allows you to open any app as root. 

To do so run
```
sudo gedit
```
OR
```
sudo mousepad
```
To fix run
```
xhost +si:localuser:root
```
