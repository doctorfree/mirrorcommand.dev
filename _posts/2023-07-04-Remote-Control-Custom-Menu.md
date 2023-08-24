---
title: Remote Control Custom Menu
author: doctorfree
date: 2023-07-04 16:20:00 +0800
tags: [magicmirror, remote control, custom menu]
pin: true
img_path: "/posts/20230720"
---

## Overview

The `mirror` command line utilities can be integrated into a custom
[MMM-Remote-Control](https://github.com/Jopyth/MMM-Remote-Control){:target="_blank"}{:rel="noopener noreferrer"} menu.
In this way the MMM-Remote-Control module can be extended to perform
many additional actions including taking a screenshot, rotating the
display, and controlling playback of video. This can, for example,
allow you to use your phone to control the MagicMirror while standing
in front of the mirror, away from your computer. Particularly handy
for taking screenshots.

The MMM-Remote-Control module provides some documentation on creating
a custom menu but it is currently incomplete. To add custom commands
to MMM-Remote-Control using the `mirror` command:

- Install the MirrorCommand utilities
    - See the MirrorCommand [Installation section](https://mirrorcommand.lazyman.dev/install)
- Create a custom JSON configuration file in the MagicMirror config folder
- Modify the config section of the MMM-Remote-Control module entry in the MagicMirror `config.js`

## Custom JSON

Below is an example custom JSON MMM-Remote-Control configuration file. Create a file like this and copy it to your MagicMirror config folder. You can name it anything you like but the default naming convention is `custom_menu.json`.

This file creates the additional custom menu entries that MMM-Remote-Control will use. The current documentation for the syntax of this file only shows how to add NOTIFICATION actions. Integration with the Mirror Command Line requires custom COMMAND actions. Use the following as a guide to create a custom MMM-Remote-Control menu or simply copy it in its entirety, unmodified, as it is ready to be used in conjunction with the `mirror` command.

For each COMMAND menu entry, specify the `action` as "COMMAND" and the `content` as "command": "someUniqueStringYouMakeUp". The `command` string you use for the action must be unique and the command it represents will be defined in the MagicMirror config.js described in the next section.

### Example Custom JSON

```json
{
   "id": "custom",
   "type": "menu",
   "icon": "id-card-o",
   "text": "Mirror Command Line",
   "items": [{
         "id": "screenshot",
         "type": "item",
         "icon": "dot-circle-o",
         "text": "Screenshot",
         "action": "COMMAND",
         "content": {
            "command": "screenshotCommand"
         }
      },
      {
         "id": "rotate",
         "type": "menu",
         "menu": "custom",
         "icon": "bars",
         "text": "Rotate Screen",
         "items": [{
               "id": "rotate-right",
               "type": "item",
               "icon": "dot-circle-o",
               "text": "Rotate Screen Right",
               "action": "COMMAND",
               "content": {
                  "command": "rotateScreenRight"
               }
            },
            {
               "id": "rotate-left",
               "type": "item",
               "icon": "dot-circle-o",
               "text": "Rotate Screen Left",
               "action": "COMMAND",
               "content": {
                  "command": "rotateScreenLeft"
               }
            },
            {
               "id": "rotate-normal",
               "type": "item",
               "icon": "dot-circle-o",
               "text": "Rotate Screen Normal",
               "action": "COMMAND",
               "content": {
                  "command": "rotateScreenNormal"
               }
            },
            {
               "id": "rotate-inverted",
               "type": "item",
               "icon": "dot-circle-o",
               "text": "Rotate Screen Inverted",
               "action": "COMMAND",
               "content": {
                  "command": "rotateScreenInverted"
               }
            }
         ]
      },
      {
         "id": "video",
         "type": "menu",
         "menu": "custom",
         "icon": "bars",
         "text": "Video Playback",
         "items": [{
               "id": "play-video",
               "type": "item",
               "icon": "dot-circle-o",
               "text": "Play Video",
               "action": "COMMAND",
               "content": {
                  "command": "playVideo"
               }
            },
            {
               "id": "pause-video",
               "type": "item",
               "icon": "dot-circle-o",
               "text": "Pause Video",
               "action": "COMMAND",
               "content": {
                  "command": "pauseVideo"
               }
            },
            {
               "id": "replay-video",
               "type": "item",
               "icon": "dot-circle-o",
               "text": "Replay Video",
               "action": "COMMAND",
               "content": {
                  "command": "replayVideo"
               }
            },
            {
               "id": "next-video",
               "type": "item",
               "icon": "dot-circle-o",
               "text": "Next Video",
               "action": "COMMAND",
               "content": {
                  "command": "nextVideo"
               }
            },
            {
               "id": "hide-video",
               "type": "item",
               "icon": "dot-circle-o",
               "text": "Hide Video",
               "action": "COMMAND",
               "content": {
                  "command": "hideVideo"
               }
            },
            {
               "id": "show-video",
               "type": "item",
               "icon": "dot-circle-o",
               "text": "Show Video",
               "action": "COMMAND",
               "content": {
                  "command": "showVideo"
               }
            }
         ]
      }
   ]
}
```

## MMM-Remote-Control config

Add the following to the MMM-Remote-Control module config in the MagicMirror config folder's `config.js`. If you have configured an API Key for MMM-Remote-Control then modify the `apiKey` setting with your key. The `customCommand` settings correspond to the `command` settings used for the actions in the `custom_menu.json` file described above. Modify these to match those used in your `custom_menu.json` file. If you used the sample JSON as provided above then no further modifications to this config section will be necessary.

Add the following to the MMM-Remote-Control module entry in `MagicMirror/config/config.js`:

```javascript
    config: {
        apiKey: 'xxxx_Your-API-Key-Here_xxxx',
        customCommand: {
            shutdownCommand: '/usr/local/bin/shutdown',
            rebootCommand: '/usr/local/bin/reboot',
            monitorOnCommand: 'vcgencmd display_power 1',
            monitorOffCommand: 'vcgencmd display_power 0',
            screenshotCommand: '/usr/local/bin/mirror screenshot',
            rotateScreenRight: '/usr/local/bin/mirror rotate right',
            rotateScreenLeft: '/usr/local/bin/mirror rotate left',
            rotateScreenNormal: '/usr/local/bin/mirror rotate normal',
            rotateScreenInverted: '/usr/local/bin/mirror rotate inverted',
            playVideo: '/usr/local/bin/mirror playvideo',
            pauseVideo: '/usr/local/bin/mirror pausevideo',
            replayVideo: '/usr/local/bin/mirror replayvideo',
            nextVideo: '/usr/local/bin/mirror nextvideo',
            hideVideo: '/usr/local/bin/mirror hidevideo',
            showVideo: '/usr/local/bin/mirror showvideo',
        },
        showModuleApiMenu: true,
        secureEndpoints: true,
        customMenu: "custom_menu.json",
    }
```

Restart MagicMirror and open `http://YourMagicMirrorIP:8080/remote.html` in a browser. You should see a "Mirror Command Line" menu entry in the main MMM-Remote-Control menu (or whatever you set the top-level custom menu "text" property to in the JSON above). Clicking on this entry should bring up the custom menus defined in your `custom_menu.json` file.
