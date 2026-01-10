## Running Project Rubi-Ka in Linux using wine


I am using Arch linux and the vanilla wine that is already in the repositories.
Arch uses quite recent versions of software but I don't see why this should not work on most distros.


Install wine and winetricks


Create a new wine prefix for PRK
```
$ env WINEPREFIX=~/.wine-prk wineboot -u
```


Run winecfg and under the "graphics" tab enable virtual desktop and set it to the resolution of your screen
```
$ WINEPREFIX=~/.wine-prk winecfg
```


From now on the virtual desktop will pop up every time you run a wine or winetricks command. This is normal.


Install fonts and dxvk using winetricks
```
$ WINEPREFIX=~/.wine-prk winetricks allfonts dxvk
```


Download the [Linux client for PRK](https://prk-vault.nyc3.digitaloceanspaces.com/launcher/linux.zip)


Extract the archive, this should create a "linux" folder with PRK executables in it
```
$ unzip linux.zip
```


Move the "linux" folder to the C: drive inside the wine prefix
```
$ mv linux ~/.wine-prk/drive_c/
```


Change directory to where the PRK launcher is
```
$ cd ~/.wine-prk/drive_c/linux
```


Make the PRK.Launcher executable
```
$ chmod a+x ./PRK.Launcher
```


Run the Launcher (Working directory must be where the Launcher is)
```
$ WINEPREFIX=~/.wine-prk ./PRK.Launcher
```


From within the launcher set the path for the compatibility tool (/usr/bin/wine and ~/.wine-prk)

The following game settings seems to work well: Device - HAL, Resolution - what your screen is, Mode - Fullscreen, DirectX Layer - None

Using other settings than this may or may not work for you.

Then download, patch and run the game




