# Roblox 0.3.368.0

This is a newly discovered build of Roblox, compiled in March of 2007.<br/>
To run the client, download this repository as a zip and extract it.<br/>
The executable can be found in the `client` folder!


## Credits ##

* **Konotsu**, **Matsu**, **TheMiningBoyAlpha**/**TheSpiderLuke**: Finding client and being the first ones to get it running.
* **CloneTrooper1019**: Making this repository, public awareness, fixed places and wrote the game join scripts.
* **Nukley**: Added more accurate player animations, Helped with structuring the client folder and replacing non-canonical files with authentic ones.
* **XIXi**: Provided missing features for the game join scripts.
* **pizzaboxer**: Got the ThumbnailGenerator into a functional state and wrote documentation.
* **Vulpovile**: Fixed player sounds and provided initial reconstructed files, announced the build's discovery to our Discord communities.

>**Please do not contact any contributors for assistance in getting this to work. If you intend to mess with this build, you're on your own.
We have done all that we can to provide enough information to people who have enough technical knowledge to get this up and running.**

# IMPORTANT: Antivirus Disclaimer #

Some users have been reporting that their antivirus programs are flagging this as malicious. This is due to the current antivirus ecosystem flagging effectively everything that isn't signed code as malicious if it performs any activities that are deemed as such, even if those actions are non-malicious (such as writing to the Windows registry, writing files in the background, or downloading any data on the network). 

Roblox unfortunately did not sign the executable when this was compiled in March of 2007, hence why these warnings are appearing.
This should be cleared once the executable is submitted for manual analysis, but for now you'll need to make an exception if any sort of anti-virus alarm is raised.

**IN SUMMARY**, this is a misdiagnosis and there should be nothing to worry about. But please do make sure to heed some of the security warnings listed below.

# Requirements #

For this to execute correctly, you need to make sure your system has the `Microsoft Visual C++ 2005 Service Pack` installed. This pack was standard in most older systems, but newer systems are no longer bundled with it.<br/>

You can find it here:<br/>
https://www.microsoft.com/en-us/download/details.aspx?id=26347

**(Note**: You will need the x86 pack. It may also help to have the x64 pack as well.)

# Content Disclaimer #

Some of the files here are not the authentic original files, this is just an approximate reconstruction from files we do have. The executable is authentic, but some files present in the client folder (i.e. `ReflectionMetadata.xml` and the `content` folder), may not be 100% accurate to what was in the client as of March 2007. We are continuously working to get it as close as possible.

# WARNING: DO NOT CONNECT TO UNTRUSTED SERVERS #

This build does support hosting and connecting to servers, but there's a non-zero chance this build has exploitable bugs that can be used for **remote code execution** on your machine.<br/>

If you do try and take advantage of the multiplayer functionality in this build, make sure you only connect to servers that are trustworthy. Otherwise it isn't worth doing.<br/>

**A publicly shared server >>IN PARTICULAR<< is very likely dangerous to connect to. Even if the server host isn't doing anything malicious, a malicious client could connect to the game and potentially exploit some buffer overflow in the network protocol to perform remote code execution on your machine. Private servers where every participant is known and trustworthy are probably okay, but just remember to be careful!<br/>**

You have been warned, be smart and have fun :)!

# Download Link #
https://github.com/CloneTrooper1019/Roblox_0.3.368.0/archive/main.zip

# Basic Commands #

You can enable the command bar by navigating to the bar at the top of the screen and clicking `View -> Toolbars -> Command`
Here are some basic commands that can be used to do various things in this build:

## Opening Places ##

* Open Crossroads:<br/>
`game:load("rbxasset://../../extra/places/Crossroads.rbxl")`

* Open Happy Home in Robloxia:<br/>
`game:load("rbxasset://../../extra/places/HappyHomeInRobloxia.rbxl")`

* Open Roblox HQ:<br/>
`game:load("rbxasset://../../extra/places/RobloxHQ.rbxl")`

* Open Tabula Rasa:<br/>
`game:load("rbxasset://../../extra/places/TabulaRasa.rbxl")`

## Starting Game Sessions ##

* Start a Play Solo session:<br/>
`loadfile("rbxasset://../../extra/scripts/PlaySolo.lua")()`

* Start a localhost server.<br/>
`loadfile("rbxasset://../../extra/scripts/StartServer.lua")()`

* Connect to a localhost server:<br/>
`loadfile("rbxasset://../../extra/scripts/StartPlayer.lua")()`

## Manual Actions ##

* Create a player manually:<br/>
`game.Players:createLocalPlayer(0)`

* Load your player's character manually:<br/>
`game.Players.LocalPlayer:LoadCharacter()`

* Run the game manually:<br/>
`game:service("RunService"):run()`

* Reset the DataModel to an empty state:<br/>
`game:clearContent()`

# Bloom and Depth of Field #
Just like the [mid-2007 client](https://github.com/CloneTrooper1019/Roblox-2007-Client#enabling-bloom-and-depth-of-field-effects), you can enable depth of field and bloom. It's still disabled, so you will need to hex-edit the executable with a program such as HxD. To enable either one of these, change the following offsets from 00 to 01.
- Bloom: `F2401`
- Depth of Field: `F240E`

![Depth of field and bloom in a hex editor](https://user-images.githubusercontent.com/62524115/111388579-8a0bdf80-867d-11eb-9bf0-f36fafc4ca1a.png)

# ThumbnailGenerator Support #

This build features the `ThumbnailGenerator` service, which is still used by Roblox to this day for rendering avatars and game thumbnails!<br/>
While it only exists on Roblox's backend server today, it happened to exist in this build for some reason.<br/>
It has the following API definition:

```
Class ThumbnailGenerator : Instance
    Function string ThumbnailGenerator:click(string fileType, int cx, int cy, bool hideSky)
    Function string ThumbnailGenerator:clickTexture(string fileType, int cx, int cy, bool hideSky)
    Property int ThumbnailGenerator.GraphicsMode
```

Note that this service **does not work** immediately, it will crash Roblox if it isn't setup correctly.<br/>
You must copy the following files into the `client` directory of this repository:

- `extra/Mesa-6.5.2/GLU32.DLL` -> `client/GLU32.DLL`
- `extra/Mesa-6.5.2/OPENGL32.DLL` -> `client/OPENGL32.DLL`
- `extra/Mesa-6.5.2/OSMESA32.DLL` -> `client/OSMESA32.DLL`

## IMPORTANT NOTES ##

- When these DLL files are active, the rendering speed will slow to a crawl depending on the resolution of the game window.
	- This cannot be avoided as this forces rendering to software rendering.
	- Remove the DLLs from the `client` directory when you aren't using them!
- Shadows **must be disabled** or Roblox will crash when opening a new place.
- Upon using `ThumbnailGenerator:click`, the crash dialog may show (though usually you can still interact with the client).
- MesaLib is very prone to rendering bugs (text appearing as black boxes, etc), though these don't affect the outcome of the thumbnail.

- Arguments for `ThumbnailGenerator:click` have the following constraints:
  - `fileType` can be `"PNG"`, `"JPG"`, `"TGA"`, `"BMP"`, `"PCX"` or `"ICO"` 
  - `cx` is the width of the thumbnail (max is 4096)
  - `cy` is the height of the thumbnail (max is 4096)
  - `hideSky` will make the sky transparent and adjust the camera angle if set to true, and will keep the sky and original camera angle if set to false
 - The string returned by the function is the image encoded in base64. Use a base64 to image converter like this one [here](https://codebeautify.org/base64-to-image-converter).

Example usage (renders an avatar thumbnail):

```lua
if not game.Players.LocalPlayer then
	game.Players:createLocalPlayer(0)
end

game.Players.LocalPlayer:LoadCharacter()
print(game:service("ThumbnailGenerator"):click("PNG", 420, 420, true))
```
