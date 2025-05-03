# Sign Artist

Oxide plugin for Rust. Load custom images to signs from a remote URL.

**Note:** The code that handles the resizing requires gdiplus.dll.  This should be present on Windows machines.  But, on Linux, you will most likely have to install the package libgdiplus.  Because this library depends on mono you will also have to add the mono repository to your system.  Please refer to the [FAQ](https://umod.org/community/sign-artist/5685-sign-artist-faq-from-oxide-my-one-critical-addition) to see how to do this.

This plugin allows players with the appropriate permission to use images from the internet to display on signs.

## Configuration

```json
{
  "Time in seconds between download requests (0 to disable)": 0,
  "Maximum concurrent downloads": 5,
  "Maximum distance from the sign": 3,
  "Maximum filesize in MB": 1.0,
  "Enforce JPG file format": false,
  "JPG image quality (0-100)": 85,
  "Enable logging file": false,
  "Enable logging console": false,
  "Enable discord logging": false,
  "Discord Webhook": "",
  "Avatar URL": "https://i.imgur.com/dH7V1Dh.png",
  "Discord Username": "Sign Artist"
}
```

A few notes about some configuration values:

* `Maximum concurrent downloads` is also used for a separate restore queue, so the plugin will never try to restore more signs at the same time than the value entered here.
* `JPG image quality` is a numeric value between 0 and 100 (inclusive) that defines the jpeg image quality when the image is saved to storage.

## Permissions

* `signartist.url`  
  Allows the player to use the /sil command
* `signartist.text`  
  Allows the player to use the /silt command
* `signartist.restore`  
  Allows the player to use the /silrestore command
* `signartist.ignoreowner`  
  Allows the player to use the /sil and /silt commands when he does not have building permissions
* `signartist.ignorecd`  
  Allows the player to use the /sil and /silt commands without trigger a cooldown.
* `signartist.raw`  
  Allows the player to specify the `raw` argument to ignore the jpeg enforcement when it is enabled in the config
* `signartist.restoreall`  
  Allows the player to specify the `all` argument for /silrestore to restore all signs at once.

## Commands

### Chat Commands

* `/sil <url> [raw]`  
  Download the image from the url to the server and display it on the sign you are currently looking at. Specifying the \`raw\` argument allows you to ignore jpeg enforcement if that is enabled in the config file.
* `/silt <message> [<fontsize: number>] [<color: hex value>] [<bgcolor: hex value>] [raw]`  
  Downloads a generated image with the given text and optional fontsize, color, bgcolor to be displayed on the sign you are currently looking at. Specifying the `raw` argument allows you to ignore jpeg enforcement if that is enabled in the configuration file.
* `/silrestore [all] [raw]`  
  Restores an image on the sign that was broken during the last Rust update. Specifying the `all` argument will restore all signs on the server. Specifying the `raw` argument allows you to ignore jpeg enforcement if that is enabled in the configuration file.

* `/sili`  
Adds currently held item icon to sign or frame. Use  `/sili default` to add that items default skin. Note that un-approved workshop skins will upload the first image in the workshop preview.

(Keep in mind, if the sign is from a copy/pasted building you will have to save it again to correctly save the files for copy/paste for future use, unless you want to restore after every paste ;) )

### Console Commands

These console commands do the exact same thing as the chat commands and can only be executed from the ingame console. They were added to allow input (long urls and such) that can't be send through chat.

* `sil <url> [raw]`
* `silt <message> [<fontsize: number>] [<color: hex value>] [<bgcolor: hex value>] [raw]`

### Examples

**Load from URL**
`/sil https://assets.umod.org/images/umod-gray-nomargin.png`

**Load from local file**
`/sil file:///C:/Windows/test.png`
`/sil file:///c:\img\test.png` (might work better).  On Windows, be sure to work in a directory that's not restricted for some reason, e.g. C:\Windows.

## Discord Webhook

A [discord webhook](https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks) can be used to log images which are uploaded to signs. You will need to edit the values in your config to enable the webhook.

```json
{  
"Enable discord logging": true,
   "Discord Webhook": "PUT YOUR WEBHOOK HERE",
    "Avatar URL": "https://i.imgur.com/dH7V1Dh.png",
    "Discord Username": "Sign Artist"
    }
```

![DiscordMassage](https://i.imgur.com/QUVSJMX.jpg)

## For Developers

### API

```csharp
void API_SkinSign(BasePlayer player, Signage sign, string url, bool raw = false)
```

```csharp
void API_SignText(BasePlayer player, Signage sign, string message, int fontsize = 30, string color = "FFFFFF", string bgcolor = "000000")
```

### Hooks

```csharp
void GetImageRotation(BaseEntity sign)
```

```csharp
void OnImagePost(BasePlayer player, string url)
```

```csharp
void OnSignUpdated(Signage sign, BasePlayer  player)
```

## Credits

* **Bombardir**, the original author of this plugin
* **Nogrod**, for maintenance,
* **Mughisi**, for a re-write
* **rfc1900**, for helping maintain the plugin,
* **MJSU**, for patches and suggestions
