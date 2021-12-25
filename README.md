# Modding-Bannerlord-First-Look

## Prerequisites

1. [Visual Studio Code](https://code.visualstudio.com/)
2. [.NET Framework Developer Pack](https://dotnet.microsoft.com/en-us/download/dotnet-framework/net472)
3. [Visual Studio](https://visualstudio.microsoft.com/vs/)

## Project
This is a first look at modding Mount & Blade II: Bannerlord. I want to use modding as a way to learn C# and this page will help me learn and record concepts. These few couple mods will be simple concepts. One mod will add a main menu button that will print text on the main menu when clicked. The other mod will simply add custom text to the in-game tip menu.

## Setup
To setup these mods I will first need to go to the Mount & Blade II: Bannerlord folder loacted in "steamapps". 
For me this location will be: S:\Steam\steamapps\common\Mount & Blade II Bannerlord
Inside the "Mount & Blade II Bannerlord" folder I will navigate to the "Modules" folder. Inside the Modules folder I will create a folder called "ModTest", which will act as the folder for these mods. Inside the "ModTest" folder I will create another folder called "bin".

The "bin" folder is meant to hold binary files, which for these mods will hold all the dependencies for the mod. These dependencies are the dll files associated with the Mount & Blade II: Bannerlord.

Dll files are short for Dyamic Link Library, which is a type of file that contains instructions that other programs can call upon to do certain things. They are similar to exe files, except they can't be run directly, but instead must be calle dupon by other code that is already running. In the case of these mods I will add all the TaleWorlds.dll files as references. This isn't normally needed, but will be fine for this project.

Inside the "ModTest" folder I will also create a new file called "SubModule.xml". This file will inform the launcher of my mod. Inside the "bin" folder I will create a folder called "Win64_Shipping_Client". This will be the output path for the build after compiling code files.

The file structure inside the Module folder should look like this:

Modules/
├─ ModTest/
│  ├─ bin/
│  │  ├─ Win64_Shipping_Client/
│  ├─ SubModule.xml
