# GXP Linux Fix [VSCode]
 A linux fix for the GXP engine using VSCode and proton
 
This repository has everything i put together to get gxp running on linux using vscode and proton through lutris and/or steam to run the game.

NOTE that you do need the f5anything extention to run the game through proton unless you found another method to do so.
And remove "(config)" from the .gitignore, everything should be pasted into a folder/directory above the engine's workfolder.
Also remove "(linux)" or "(mac)" from the GXPEngine.exe.config in Debug.

For Lutris:
- you manualy add a game and select a locally installed game, set the gane runner to wine
- in game options, navigate your way to the exe (usually in bin/Debug)
- you can adjust the settings in runner options as needed and set wine version to proton
- in system options, enable the advanced mode next to the save button in th top right
and scroll down to Text based games and enable cli mode

save!


make sure you also have mono-complete installed for your system

if all went well you should be able to run and debug your game!