# Running Project Rubi-Ka in Linux using wine
<br/>

I am using Arch linux and the vanilla wine that is already in the repositories.
Arch uses quite recent versions of software but I don't see why this should not work on most distros.

Lines starting with $ indicates a command to be executed in a shell, the $ should not be included in the command.

<br/><br/>
Install wine and winetricks

<br/><br/>
Create a new wine prefix for PRK
```
$ env WINEPREFIX=~/.wine-prk wineboot -u
```


Run winecfg and under the "graphics" tab enable virtual desktop and set it to the resolution of your screen
```
$ WINEPREFIX=~/.wine-prk winecfg
```


From now on the virtual desktop will pop up every time you run a wine or winetricks command. This is normal.

<br/><br/>
Install fonts and dxvk using winetricks (This will take some time to complete)
```
$ WINEPREFIX=~/.wine-prk winetricks allfonts dxvk
```


Note: Strictly speaking, the game doesn't need dxvk, but dgVoodoo2 which we will install later does

<br/><br/>
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


From within the launcher set the path for the compatibility tool (/usr/bin/wine and ~/.wine-prk) and save.

Note: The Launcher will sometimes crash at this step. If it does, save some other settings like the resolution, and exit the Launcher.
That should generate the LauncherSettings.json file where we can update the path to the compatibility tool manually.

Example of LauncherSettings.json: `..."CompatibilityToolEnabled":true,"CompatibilityToolPath":"/usr/bin/wine","CompatibilityToolPrefix":"~/.wine-prk"...`
<br/><br/><br/>
Moving on, the following game settings seems to work well: Device - HAL, Resolution - what your screen is, Mode - Fullscreen, DirectX Layer - None

Using other settings than this may or may not work for you.

Then download, patch and run the game.

<br/><br/>
So far so good, but enabling FPS in game (Ctrl+Alt F) shows the game running with only 40-70 FPS,
compared to Windows 11 which is running the game consistently with 100 FPS (max for this game).
<br/><br/>
Lets fix that

<br/>

## How to apply dgVoodoo2 version 2.79.3 to PRK
<br/>

Note: Newer versions of dgVoodoo2 does not work with wine


Rename the file `~/.wine-prk/drive_c/windows/syswow64/ddraw.dll` to `~/.wine-prk/drive_c/windows/syswow64/ddraw.dll_`
```
$ mv ~/.wine-prk/drive_c/windows/syswow64/ddraw.dll ~/.wine-prk/drive_c/windows/syswow64/ddraw.dll_
```

Copy the 4 files under `~/.wine-prk/drive_c/linux/dist/directx/dgVoodoo2_79_3/` 

and paste them into the Windows system folder `~/.wine-prk/drive_c/windows/syswow64/`
```
$ cp ~/.wine-prk/drive_c/linux/dist/directx/dgVoodoo2_79_3/* ~/.wine-prk/drive_c/windows/syswow64/
```

<br/><br/>

cd to `~/.wine-prk/drive_c/windows/syswow64/` and make the dgVoodooCpl.exe file executable
```
$ chmod a+x ./dgVoodooCpl.exe
```

then run it

```
$ WINEPREFIX=~/.wine-prk ./dgVoodooCpl.exe
```

Here I selected my nvidia graphics card as adapter on the first tab and enabled the dgVoodoo watermark on the third tab.

<br/><br/>

Start winecfg
```
$ WINEPREFIX=~/.wine-prk winecfg
```


Under the "Libraries" tab add a new "override for library" called ddraw (accept the warning)

It should show up in the list of overrides as `ddraw (native, builtin)`

Do the same for d3dimm
<br/><br/>

Make sure the 4 files (DDraw.dll, D3DImm.dll, dgVoodooCpl.exe and dgVoodoo.conf) are not present under the AO client directory `~/.wine-prk/drive_c/linux/client`,
just to be sure there are no conflicts with the system files we just installed.
<br/><br/>

Now cd to the PRK Launcher and run it
```
$ WINEPREFIX=~/.wine-prk ./PRK.Launcher
```


Set the game to run in Windowed mode and set the DirectXLayer to "None"


With dgVoodoo2 enabled, running the game in Fullscreen mode makes the game window tiny for some reason so Windowed mode (with or without border) is the way to go for now.


At this point the game should run with full performance and the logo should be visible in the bottom right corner

<br/>

## Here is a simple bash script that can be used to start the game
<br/>

```
#!/usr/bin/sh
cd ~/.wine-prk/drive_c/linux
WINEPREFIX=~/.wine-prk 
./PRK.Launcher
```

Save it as prk.sh and make it executable
```
$ chmod a+x ./prk.sh
```
