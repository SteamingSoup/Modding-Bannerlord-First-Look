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
```
Modules/
├─ ModTest/
│  ├─ bin/
│  │  ├─ Win64_Shipping_Client/
│  ├─ SubModule.xml
```

Now that the setup is complete I can continue to making the mods.

## Adding main menu button
The first things that needs to be done is editing the "SubModule.xml" file. Open the file in visual studio code and add the following code:
```
 <Module>
     <Name value="Mod Test"/>
     <Id value="ModTest"/>
     <Version value="v1.0.0"/>
     <SingleplayerModule value="true"/>
     <MultiplayerModule value="false"/>
     <DependedModules>
         <DependedModule Id="Native"/>
         <DependedModule Id="SandBoxCore"/>
         <DependedModule Id="Sandbox"/>
         <DependedModule Id="CustomBattle"/>
         <DependedModule Id="StoryMode" />
     </DependedModules>
     <SubModules>
         <SubModule>
             <Name value="ModTest"/>
             <DLLName value="ModTest.dll"/>
             <SubModuleClassType value="ModTest.Main"/>
             <Tags>
                 <Tag key="DedicatedServerType" value="none" />
                 <Tag key="IsNoRenderModeElement" value="false" />
             </Tags>
         </SubModule>
     </SubModules>
     <Xmls/>
 </Module>
 ```
 
 Under Module the values for Name and Id was changed to "Mod Test" and "ModTest", respectively. Under SubModule the values for the Name, DLLName, and SubModuleClassType was changed to "ModTest", "ModTest.dll". and "ModTest.Main", respectively. All other values, Ids, or keys can be left as is.
 
 Here is a description of each element in the XML:
 * Name - The name of your Module.
 * Id - The id of your Module (do not use spaces).
 * Version - The current version of your Module.
 * SinglePlayerModule - Whether or not your module is meant for Single Player mode.
 * MultiPlayerModule - Whether or not your module is meant for Multi Player mode.
 * DependedModules - Modules that your module requires in order to function properly.
 * SubModules - The SubModules (DLLs) that your modules consists of.
 * Xmls - Contains Paths to XML files in the ModuleData Folder(s)

Save this file then proceed to open visual studio. In visual studio go ahead and create a new project and choose "Class Library (.NET Framework) as the project template. The project name should match the actual module name, which in this case is ModTest, then click create.

In visual studio go to the "Project" tab at the top, then go to "ModTest properties". In the left hand panel go to "Build" then go down to "Output path". Click "browse" next to Output path to designate the folder you will assign as the output path. This folder will be the "Win64_Shipping_Client" made earlier.

In visual studio on the right you should see "Class1.cs" under ModTest. Rename this source code file to "Main.cs".

To add the dependencies go to "Project" tab at then click "add reference". Click "Browse" then go to the Mount & Blade II Bannerlord folder. Click on the "bin" folder then the "Win64_Shipping_Client" folder. Note that these are not the folders located in the ModTest folder. From here add all the TaleWorlds binaries. From here click "Build" at the top of visual studio then click "Build Solution". This will build to the output path that was setup earlier.

To makes a main menu item that will output a message on the main menu we will use the following code:
```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using TaleWorlds.Core;
using TaleWorlds.Localization;
using TaleWorlds.MountAndBlade;

namespace ModTest
{
    public class Main : MBSubModuleBase
    {
        protected override void OnSubModuleLoad()
        {
         Module.CurrentModule.AddInitialStateOption(new InitialStateOption("Message",
            new TextObject("Message", null),
            9990,
            () => { InformationManager.DisplayMessage(new InformationMessage("Hello World!")); },
            () => { return (false, null); }));
        }
    }
}
```
The MBSubModuleBase class is the entry point to the code and it will handle the loading of the module. Protected override void OnSubModuleLoad() sets up an override for the OnSubModuleLoad() inherited method. The rest of the code simply adds a new object that we called "Message", which will show up on the main menu as "Message", and when this is clicked it will display a message that says "Hello World!". 

Here are the results:

Make sure to activate the mod in the mod menu. It should show up as Mod Test.

![ModTest](/images/ModTest.png)


And on the main menu we should see a "Message" button and clicking it prints "Hello Worlds!" onto the main menu.

![Message](/images/message_bannerlord.png)

super simple mod, but it is a start. Let's move on to adding a message into the in-game tip menu.

## Adding text to in-game tip menu
This mod is simply editing an xml file within the game directory. The xml file in question is the "EscapeMenu.xml". This file can be found in the "Mount and Blade II Bannerlord folder where you will navigate to > Modules > Native > GUI > Prefabs. Copy the "EscapeMenu.xml" file then go to the "ModTest" folder in "Modules". Within the "ModTest" folder make a new folder called "GUI". In the "GUI" folder make a new folder called "Prefabs", then paste the "EscapeMenu.xml" file here. This folder will house all the UI information for the mod.

The file structure should now look like this:
```
Modules/
├─ ModTest/
│  ├─ bin/
│  │  ├─ Win64_Shipping_Client/
│  ├─ GUI/
│  │  ├─ Prefabs/
│  │  │  ├─ EscapeMenu.xml
│  ├─ SubModule.xml
```

Open the "EscapeMenu.xml" file with visual studio code. I will edit the xml where it says "GameTip.Text" and change the Text value to say "This is a modded tip". The file should look like this now:
```
<Prefab>
  <Constants>
    <Constant Name="EscapeMenu.Background.Width" BrushName="EscapeMenu.Background" BrushLayer="Default" BrushValueType="Width"/>
    <Constant Name="EscapeMenu.Background.Height" BrushName="EscapeMenu.Background" BrushLayer="Default" BrushValueType="Height"/>

    <Constant Name="ContextButton.Width" BrushName="ButtonBrush2" BrushLayer="Default" BrushValueType="Width"/>
    <Constant Name="ContextButton.Height" BrushName="ButtonBrush2" BrushLayer="Default" BrushValueType="Height"/>
  </Constants>
  <Window>
    <Widget WidthSizePolicy="StretchToParent" HeightSizePolicy="StretchToParent" Sprite="BlankWhiteSquare_9" Color="#000000FF" AlphaFactor="0.4">
      <Children>

        <Widget WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="!EscapeMenu.Background.Width" SuggestedHeight="!EscapeMenu.Background.Height" HorizontalAlignment="Center" VerticalAlignment="Center" Sprite="SPGeneral\EscapeMenu\escape_panel">
          <Children>
            <ListPanel DataSource = "{MenuItems}" Id="MyInnerPanel" StackLayout.LayoutMethod = "VerticalBottomToTop" WidthSizePolicy = "StretchToParent" HeightSizePolicy = "CoverChildren" MarginTop="100" MarginBottom="115" >
              <ItemTemplate>
                <Widget WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="!ContextButton.Width" SuggestedHeight="!ContextButton.Height" HorizontalAlignment="Center" VerticalAlignment="Center" MarginBottom="30" UseSiblingIndexForNavigation="true">
                  <Children>
                    
                    <EscapeMenuButtonWidget DoNotPassEventsToChildren="true" Command.Click="ExecuteAction" WidthSizePolicy="StretchToParent" HeightSizePolicy="StretchToParent" HorizontalAlignment="Center" VerticalAlignment="Center" Brush="ButtonBrush2" PositiveBehaviorBrush="ButtonBrush1" IsDisabled="@IsDisabled" IsPositiveBehaviored="@IsPositiveBehaviored">
                      <Children>
                        <TextWidget WidthSizePolicy="StretchToParent" HeightSizePolicy="StretchToParent" PositionYOffset="1" Text="@ActionText" Brush="OverlayPopup.ButtonText" ClipContents="false"/>
                      </Children>
                    </EscapeMenuButtonWidget>

                    <Widget WidthSizePolicy="StretchToParent" HeightSizePolicy="StretchToParent" IsVisible="@IsDisabled">
                      <Children>
                        <HintWidget DataSource="{DisabledHint}" WidthSizePolicy="StretchToParent" HeightSizePolicy="StretchToParent" Command.HoverBegin="ExecuteBeginHint" Command.HoverEnd="ExecuteEndHint"/>
                      </Children>
                    </Widget>
                    
                  </Children>
                </Widget>
              </ItemTemplate>
            </ListPanel>
          </Children>
        </Widget>


        <ListPanel DataSource="{Tips}" WidthSizePolicy="CoverChildren" HeightSizePolicy="CoverChildren" StackLayout.LayoutMethod = "VerticalBottomToTop" HorizontalAlignment="Left" VerticalAlignment="Center" MarginLeft="165" MarginBottom="250">
          <Children>
            <TextWidget WidthSizePolicy="CoverChildren" HeightSizePolicy="CoverChildren" Brush="GameTip.Title.Text" Text="@GameTipTitle" HorizontalAlignment="Center" MarginBottom="15" ClipContents="false"/>
            <!--Tip Box-->
            <BrushWidget WidthSizePolicy="Fixed" HeightSizePolicy="CoverChildren" SuggestedWidth="400" HorizontalAlignment="Center" Brush="EscapeMenu.GameTip">
              <Children>
                <Widget WidthSizePolicy="StretchToParent" HeightSizePolicy="Fixed" SuggestedHeight="150" IsEnabled="false"/>
                <RichTextWidget WidthSizePolicy="StretchToParent" HeightSizePolicy="CoverChildren" Brush="GameTip.Text" MarginTop="15" MarginBottom="15" MarginRight="55" MarginLeft="55" Text="This is a modded tip" VerticalAlignment="Center"/>
                <ButtonWidget WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="43" SuggestedHeight="56" HorizontalAlignment="Left" VerticalAlignment="Center" Command.Click="ExecutePreviousTip" Brush="ButtonRightArrowBrush1" MarginLeft="3" IsVisible="@NavigationButtonsEnabled"/>
                <ButtonWidget WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="43" SuggestedHeight="56" HorizontalAlignment="Right" VerticalAlignment="Center" Command.Click="ExecuteNextTip" Brush="ButtonLeftArrowBrush1" MarginRight="3" IsVisible="@NavigationButtonsEnabled"/>
              </Children>
            </BrushWidget>
          </Children>
        </ListPanel>

      </Children>
    </Widget>
  </Window>
</Prefab>
```

Once this is done we can load up the game with the "ModTest" mod enabled and see the tips text "This is a modded tip" in the tip box in our escape menu.
