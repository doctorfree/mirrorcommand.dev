---
layout: post
icon: fas fa-user-circle
order: 4
toc: true
post_style: page
---

## MirrorCommand Component Integration

MirrorCommand integrates with several other facilities to provide
extended and additional functionality.

## Remote access

In order to remotely access the MagicMirror command line it is necessary to
setup SSH and associated SSH keys. That configuration is outside the scope
of this document. There are a number of guides on configuring SSH access on
a variety of systems. To get started with SSH configuration on a Raspberry Pi,
see <https://www.raspberrypi.org/documentation/computers/remote-access.html>

Once SSH access is configured, the [**mm**](https://github.com/doctorfree/MirrorCommand/blob/master/remote/mm){:target="_blank"}{:rel="noopener noreferrer"} script can be installed on
remote systems and used to remotely execute the mirror script on the system
hosting MagicMirror. All arguments provided to <code>mm</code> are simply
passed along to the <code>mirror</code> script.

Alternatively, the <code>mirror</code> script can be executed directly by a
user logging in to the MagicMirror system in a Shell environment (e.g. a
terminal window). This can be accomplished remotely in a terminal window
on the remote system by executing the ssh command. For example, using iTerm2
on a Mac OS X system, execute the command:

<code>ssh -l pi IP_ADDRESS</code>

where IP_ADDRESS is the IP address of the MagicMirror system. Once logged into
the MagicMirror system, the <code>mirror</code> command can be executed at a
shell command prompt:

<pre>
pi@raspberrypi:~ $ mirror
</pre>

Additional remote capabilities are provided through integration with the
[MMM-Remote-Control](https://github.com/Jopyth/MMM-Remote-Control){:target="_blank"}{:rel="noopener noreferrer"} and
[MMM-TelegramBot](https://github.com/bugsounet/MMM-TelegramBot){:target="_blank"}{:rel="noopener noreferrer"} modules.
Accessing and controlling your MagicMirror using these facilities is
described in the following sections.

### Remote execution of mirror commands

If you wish to execute mirror commands remotely then install the convenience
script [**mm**](https://github.com/doctorfree/MirrorCommand/blob/master/remote/mm){:target="_blank"}{:rel="noopener noreferrer"} on a system with SSH access to your MagicMirror. This
script can be used to remotely execute the main mirror script.

### Remote view of MagicMirror display

If you wish to view the MagicMirror display remotely then install the convenience
script [**vncview**](https://github.com/doctorfree/MirrorCommand/blob/master/remote/vncview){:target="_blank"}{:rel="noopener noreferrer"} on a system with SSH access to your MagicMirror. This
script can be used to remotely execute a VNC server and locally execute a VNC client.

### MMM-Remote-Control integration

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
to MMM-Remote-Control using the `mirror` command, see the
[MMM-Remote-Control Wiki Page](https://gitlab.com/doctorfree/MirrorCommand/-/wikis/Remote-Control-Custom-Menu){:target="_blank"}{:rel="noopener noreferrer"}.

## MMM-TelegramBot integration

You can control your MagicMirrir with the `mirror` command executed remotely
using the Telegram app. This can allow you to control your MagicMirror from
anywhere by simply sending a message on your phone using the Telegram app.
To enable this feature, install the
[MMM-TelegramBot](https://github.com/bugsounet/MMM-TelegramBot){:target="_blank"}{:rel="noopener noreferrer"}
module, setup a Telegram Bot to send and receive MMM-TelegramBot messages,
and add MMM-TelegramBot `customCommands` configuration to the MMM-TelegramBot
config section in `config/config.js`.

### MMM-TelegramBot installation

Follow the instructions at the
[4th Party Modules Wiki](http://wiki.bugsounet.fr/en/MMM-TelegramBot){:target="_blank"}{:rel="noopener noreferrer"}
to create a Telegram Bot, install MMM-TelegramBot, and configure your
MagicMirror `config.js` to enable Telegram commands.

### MMM-TelegramBot module config

In addition to following the
[4th Party Modules Wiki Installation instructions](http://wiki.bugsounet.fr/en/MMM-TelegramBot/Installation){:target="_blank"}{:rel="noopener noreferrer"}
to install the module and setup a Telegram Bot, the config section of the
MMM-TelegramBot module entry in `config.js` must be modified to add
`customCommands`. Samples of how to do this are in the config files in
this repository. For example, see the `customCommands` entry in
[**config/config-default.js**](https://github.com/doctorfree/MirrorCommand/blob/master/config/config-default.js){:target="_blank"}{:rel="noopener noreferrer"}.

Here is the section of the `customCommands` array definition in the config
section of the MMM-TelegramBot module entry in `config.js` that defines the
`/mirror` Telegram command:

```javascript
{
    command: 'mirror',
    description: "Executes MagicMirror `mirror` command\nTry `/mirror status`.",
    callback: (command, handler, self) => {
        if (handler.args) {
            var exec = "mirror -D " + handler.args
        } else {
            var exec = "mirror -D status"
        }
        handler.reply("TEXT", "Executing command: " + exec)
        var sessionId = Date.now() + "_" + self.commonSession.size
        if (exec) {
            self.commonSession.set(sessionId, handler)
            self.sendSocketNotification("SHELL", {
                session: sessionId,
                exec: exec
            })
        }
    },
},
```

### MMM-TelegramCommands module

Configuring MMM-TelegramBot customCommands can be daunting. Use the template at
[MagicMirror/config/Templates/TelegramBot-customCommands.js](https://gitlab.com/doctorfree/MirrorCommand/-/blob/master/config/Templates/TelegramBot-customCommands.js){:target="_blank"}{:rel="noopener noreferrer"}
as a guide.

Alternatively, several custom TelegramBot commands have been configured
and enabled in the
[MMM-TelegramCommands module](https://gitlab.com/doctorfree/MMM-TelegramCommands){:target="_blank"}{:rel="noopener noreferrer"}.
To enable the MMM-TelegramCommands module commands, install this module:

```bash
cd ~/MagicMirror/modules
git clone https://gitlab.com/doctorfree/MMM-TelegramCommands.git
cd MMM-TelegramCommands
npm install
```

After installing MMM-TelegramCommands add the following to the modules array
of any `config.js` that has MMM-TelegramBot activated:

```javascript
    {
        module: 'MMM-TelegramCommands'
    },
```

See this
[simple example of MMM-TelegramCommands configuration](https://gitlab.com/doctorfree/MMM-TelegramCommands/-/blob/master/examples/config-simple.js){:target="_blank"}{:rel="noopener noreferrer"} as a guide.

### Telegram usage

Once installed and configured, you can control your MagicMirror
by sending messages in the Telegram app to your previously created Telegram Bot.
If you copied the example MMM-TelegramBot customCommands configuration in
one of the config files in this repository then you will have three new
custom Telegram commands:

- /myReboot
  - Custom reboot command which executes /usr/local/bin/myreboot
- /myShutdown
  - Custom shutdown command which executes /usr/local/bin/myshutdown
- /mirror
  - General purpose command which executes the /usr/local/bin/mirror command with any arguments supplied in the Telegram command (e.g. `/mirror info` will retrieve your MagicMirror system information)

A few examples follow:

To switch the MagicMirror config.js to the configuration file
'config/config-sample.js', issue the Telegram command:

```
/mirror sample
```

To retrieve the MagicMirror status, issue the command:

```
/mirror status
```

To update the MagicMirror installation, issue the command:

```
/mirror update
```

To list the MagicMirror active modules, issue the command:

```
/mirror list active
```

Any `mirror` command can be executed via Telegram in this manner.
See `mirror -u` for the `mirror` usage message.

## MMM-GoogleAssistant integration

MirrorCommand includes configuration files to enable voice command
control of your MagicMirror utilizing the
[MMM-GoogleAssistant](http://wiki.bugsounet.fr/en/MMM-GoogleAssistant){:target="_blank"}{:rel="noopener noreferrer"}
module. Most of the MagicMirror config files in the config subdirectory
come preconfigured with voice command support. See
[**config/config-default.js**](https://github.com/doctorfree/MirrorCommand/blob/master/config/config-default.js){:target="_blank"}{:rel="noopener noreferrer"} for a sample config
file with voice control enabled.

In addition to preconfigured config files, MirrorCommand provides several
[custom MMM-GoogleAssistant recipes](https://github.com/doctorfree/MirrorCommand/blob/master/modules/MMM-GoogleAssistant/recipes){:target="_blank"}{:rel="noopener noreferrer"}.
These include recipes to:

- [Enable `mirror` command support via voice](https://github.com/doctorfree/MirrorCommand/blob/master/modules/MMM-GoogleAssistant/recipes/MirrorCommand.js){:target="_blank"}{:rel="noopener noreferrer"}
  - Voice commands to:
    - Restart MagicMirror
    - Rotate the screen
    - Turn the screen on/off
    - Mute/unmute sound
    - Copy custom config files into config.js and restart the mirror
      - Manage MagicMirror screen position on multi-screen systems
        - Move MagicMirror to screen 1
          - Say `screen one` or `mirror screen one`
        - Move MagicMirror to screen 2
          - Say `screen two` or `mirror screen two`
        - Switch the MagicMirror screen
          - Say `screen switch` or `switch screens` or `mirror screen switch`
- [Voice management of MMM-Scenes scenes](https://github.com/doctorfree/MirrorCommand/blob/master/modules/MMM-GoogleAssistant/recipes/with-MMM-Scenes.js){:target="_blank"}{:rel="noopener noreferrer"}
  - Next scene
  - Previous scene
  - Scene by number (e.g. `scene 2`)
- [Customized reboot/restart/shutdown voice commands](https://github.com/doctorfree/MirrorCommand/blob/master/modules/MMM-GoogleAssistant/recipes/myReboot-Restart-Shutdown.js){:target="_blank"}{:rel="noopener noreferrer"}
- [Radio station play via voice](https://github.com/doctorfree/MirrorCommand/blob/master/modules/MMM-GoogleAssistant/recipes/ExtRadio.js){:target="_blank"}{:rel="noopener noreferrer"}

### Google Cloud Platform API Keys

Several MagicMirror modules require a Google Cloud Platform API. The MMM-GoogleAssistant
and MMM-GoogleMapsTraffic modules are examples of these. In order to configure
the MMM-GoogleAssistant module you will need to create a Google Action project
and enable API services for that project in the Google Cloud Platform then generate
credentials for that project. This process can seem daunting to many but once you
walk through it the process becomes more transparent. A smart young woman has
provided us with a brief and simple tutorial walkthru of the process at
[https://youtu.be/xVhqP3fBnVM](https://youtu.be/xVhqP3fBnVM){:target="_blank"}{:rel="noopener noreferrer"}.
Her video is based on the
[4th Party Modules Wiki](http://wiki.bugsounet.fr/en/MMM-GoogleAssistant/GoogleAssistantSetup){:target="_blank"}{:rel="noopener noreferrer"}
description of this process.

**NOTE:** When authorizing the YouTube access token with `npm run tokens` as the final
step in this process, I found it necessary to modify
[MMM-GoogleAssistant/install/auth_YouTube.js](https://github.com/doctorfree/MirrorCommand/blob/master/modules/MMM-GoogleAssistant/install/auth_YouTube.js){:target="_blank"}{:rel="noopener noreferrer"}
to add a console log output of the generated URL to allow access. This was necessary
in my case because I was performing the process over an SSH connection in a terminal.
This is not necessary if you are accessing the MagicMirror directly or if you have
your DISPLAY set back to the system on which you are running SSH. If you need to use
this modification, it can be found at the link above.

## [MMM-Scenes](https://github.com/MMRIZE/MMM-Scenes#readme){:target="_blank"}{:rel="noopener noreferrer"} integration

In addition to the voice control of MMM-Scenes described above, several `mirror`
commands have been added to support management of MMM-Scenes scenes via the
command line. Supported commands include:

- `mirror scene next` : display the next scene in the scenario
- `mirror scene prev` : display the previous scene in the scenario
- `mirror scene name` : display the scene named 'name'
- `mirror scene num` : display scene number 'num'
- `mirror scene info` : retrieve info on MMM-Scenes configuration
