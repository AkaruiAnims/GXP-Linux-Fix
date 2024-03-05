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



Saxion guide:
==== Dynamically linking .so files (shared libraries) for the GXPEngine

If you first build and run the GXPEngine on linux, you may get a 

	DllNotFoundException
	
Here's how you can fix that:	
(Verified for Ubuntu 16.04, MonoDevelop IDE & mono (64 bit) runtime)


The GXPEngine uses the following libraries:

	WINDOWS:		LINUX:
	glfw.dll		libglfw.so
	opengl32.dll	libGL.so
	fmodex.dll		libfmodex64-4.44.56.so
	soloud.dll		?
	
To figure out which .so files are known to your dynamic linker:

	Open a terminal
	type ldconfig -p |grep libGL

It shows something like:

	libGL.so.1 (libc6,x86-64) => /usr/lib/x86_64-linux-gnu/mesa/libGL.so.1

In that case, update the GXPEngine.exe.config file in bin/Debug to contain:

    <dllmap dll="opengl32.dll">
        <dllentry os="linux" dll="libGL.so.1" />
    </dllmap>

(If it shows libGL.so.2 or something, include that of course.)

Note: make sure to pick the x86-64 version (64 bit), libc6 (32 bit) won't work.

You can do the same for glfw and fmodex. If these libraries are not already installed (fmodex most likely isn't):

Copy the .so files from the ../bin/Debug/lib folder (part of the GXPEngine handout) to your usr/lib folder (system folder). You probably need to do sudo, something like this:

	cd /usr/lib
	sudo cp [yourGXPdirectoryhere]/bin/Debug/lib/libfmodex64-4.44.56.so .

Next, make sure that your dynamic linker is aware of these new files by running

	sudo ldconfig

Finally, in your IDE (monodevelop?), go to the Sound.cs and Core/GLContext.cs files, and uncomment the first line, to state:

	#define USE_FMOD_AUDIO

Now FmodEx is used, instead of SoLoud.

Summary:
If you did this, the GXPEngine uses the following external libraries:

	glfw.dll		
	opengl32.dll	
	fmodex.dll		
	
The .exe.config in bin/Debug file maps these to:

	libglfw.so
	libGL.so
	libfmodex64-4.44.56.so

(or something equivalent, depending on your installation).

By copying these to usr/lib, and running ldconfig, these .so files are known to your system. Then you should be able to run the GXPEngine.








