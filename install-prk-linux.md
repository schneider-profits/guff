## Running Project Rubi-Ka in Linux using wine
<br/>

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

Then download, patch and run the game.

<br/><br/>
So far so good, but enabeling FPS in game (Ctrl+Alt F) shows the game running with only 40-70 FPS,
compared to Windows 11 which is running the game consistently with 100 FPS (max for this game).

<br/>

### How to apply dgVoodoo2 version 2.79.3 to PRK
<br/>

Note: Newer versions of dgVoodoo2 does not work with wine

Rename the file `~/.wine-prk/drive_c/windows/system32/ddraw.dll` to `~/.wine-prk/drive_c/windows/system32/ddraw.dll_`


Rename the file `~/.wine-prk/drive_c/windows/syswow64/ddraw.dll` to `~/.wine-prk/drive_c/windows/syswow64/ddraw.dll_`


Copy the 4 files under `~/.wine-prk/drive_c/win/dist/directx/dgVoodoo2_79_3/` and paste them into both system folders
`~/.wine-prk/drive_c/windows/system32/` and `~/.wine-prk/drive_c/windows/syswow64/`

<br/>
I'm not sure which one of these system folders is being used, so for now lets enable both.

<br/><br/>
Rename the .dll files to all lowercase (ddraw.dll, d3dimm.dll)


cd to `~/.wine-prk/drive_c/windows/system32/` and make the dgVoodooCpl.exe file executable
```
chmod a+x ./dgVoodooCpl.exe
```

then run it

```
$ WINEPREFIX=~/.wine-prk ./dgVoodooCpl.exe
```

Here I selected my nvidia graphics card as device on the first tab and enabled the dgVoodoo Logo on the third tab.

<br/><br/>
Do the exact same procedure under the syswow64 folder.
<br/><br/>

Start winecfg
```
$ WINEPREFIX=~/.wine-prk winecfg
```


Under the "Libraries" tab add a new "override for library" called ddraw (accept the warning)

It should show up in the list of overrides as `ddraw (native, builtin)`

Make sure the 4 files are not present under the AO client directory, just to be sure.

Now run the PRK Launcher
```
$ WINEPREFIX=~/.wine-prk ./PRK.Launcher
```


Set the game to run in Windowed mode and set the DirectXLayer to "None"


Running the game in Fullscreen mode makes the game window tiny for some reason so Windowed mode is the way to go for now.


At this point the game should run with full performance and the logo should be visible in the bottom right corner
