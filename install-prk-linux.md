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
$ WINEPREFIX=~/.wine-prk winetricks arial verdana dxvk
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


Run the Launcher for the first time to make sure it works and to generate the LauncherSettings.json file, then exit the launcher without doing anything yet
```
$ WINEPREFIX=~/.wine-prk ./PRK.Launcher
```


Edit the LauncherSettings.json file and make sure the compatibility tool is set correctly, on my system wine is located under /usr/bin

Example:
> ..."CompatibilityToolEnabled":true,"CompatibilityToolPath":"/usr/bin/wine","CompatibilityToolPrefix":"~/.wine-prk"...


This last step should be doable within the launcher GUI but as of this writing that does not appear to work, so we are editing this file manually


Now run the launcher and download/patch the game client, but don't start the game yet
```
$ WINEPREFIX=~/.wine-prk ./PRK.Launcher
```


Make sure the path for the compatibility tool is correct.
You can also set the following settings: Device - HAL, Resolution - what your screen is, Mode - Fullscreen, DirectX Layer - None


These settings has been working fine on my system. Using other settings than this may or may not work for you


Now that the game should be downloaded, exit the launcher and enter the client directory
```
$ cd client
```


Rename the AwesomiumProcess.exe file so that the game are unable to find and run it
```
$ mv AwesomiumProcess.exe AwesomiumProcess.exe_
```


This will prevent windows inside the game to load online web content, something that would always crash the game for me


At this point the game should run smoothly


Change directory back to where the launcher is (this is necessary), and run the game
```
$ cd ..
$ WINEPREFIX=~/.wine-prk ./PRK.Launcher
```
