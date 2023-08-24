---
layout: post
icon: fas fa-user-circle
order: 3
toc: true
post_style: page
---

## MirrorCommand Documentation

### Man Pages

Many MirrorCommand commands have manual pages. Execute `man <command-name>`
to view the manual page for a command. The `mirror` frontend is the primary
user interface for the MirrorCommand commands and the manual page for
`mirror` can be viewed with the command `man mirror`. Most commands also have
help/usage messages that can be viewed with the **-u** argument option,
e.g. `mirror -u`.

The manual page for the primary MirrorCommand user interface, `mirror`,
can be viewed by executing the command:

- `man mirror`

Manual pages for these MirrorCommand commands can be viewed by executing
any of the following commands (click to view the man page online):

- [man mirror](https://gitlab.com/doctorfree/MirrorCommand/-/blob/master/markdown/mirror.1.md){:target="_blank"}{:rel="noopener noreferrer"}
- [man 5 mirrorkeys](https://gitlab.com/doctorfree/MirrorCommand/-/blob/master/markdown/mirrorkeys.5.md){:target="_blank"}{:rel="noopener noreferrer"}
- [man set_asound_conf](https://gitlab.com/doctorfree/MirrorCommand/-/blob/master/markdown/set_asound_conf.1.md){:target="_blank"}{:rel="noopener noreferrer"}
- [man mmscene](https://gitlab.com/doctorfree/MirrorCommand/-/blob/master/markdown/mmscene.1.md){:target="_blank"}{:rel="noopener noreferrer"}
- [man mmscreen](https://gitlab.com/doctorfree/MirrorCommand/-/blob/master/markdown/mmscreen.1.md){:target="_blank"}{:rel="noopener noreferrer"}
- [man showkeys](https://gitlab.com/doctorfree/MirrorCommand/-/blob/master/markdown/showkeys.1.md){:target="_blank"}{:rel="noopener noreferrer"}
- [man vol](https://gitlab.com/doctorfree/MirrorCommand/-/blob/master/markdown/vol.1.md){:target="_blank"}{:rel="noopener noreferrer"}
- [man websnap](https://gitlab.com/doctorfree/MirrorCommand/-/blob/master/markdown/websnap.1.md){:target="_blank"}{:rel="noopener noreferrer"}

### Usage

The `mirror` shell script is installed on any MagicMirror system that you want
to utilize for command line control of your MagicMirror. Remote execution of
the `mirror` command line script may be accomplished by using the "mm"
convenience script on a remote system with SSH access to your MagicMirror.

Here is the current output of "mirror -u" which displays a usage message.

```
Usage: mirror <command> [args]

Where <command> can be one of the following:
    info <temp|mem|disk|usb|net|wireless|screen>
    list <active|installed|configs>
    rotate <right|left|normal|inverted|default>
    scene <next|prev|info|name|number>
    screen <on|off|info|status>
    stop|start|restart|mute|unmute|screenshot|reboot|shutdown
    playvideo|pausevideo|nextvideo|replayvideo|hidevideo|showvideo
    update [modules]
    vol <percent>|mute|unmute|save|restore|get
    dev | getb | setb <num> | select | status <all> | youtube
    artists_dir, models_dir, photogs_dir
    ac|ar <artist>, jc|jr <idol>, mc|mr <model>, pc|pr <photographer>, wh|whrm <dir>

Specify a config file to use by executing a command of the form:

 mirror <name>

where <name> is one of:
  all Artists art background bikini blank calendar candy covid
  crypto openweather default face fractals gif iframe instagram ironman
  JAV Models nature networkcols network networktest news nokeys normal
  owls Photographers pics portal radar rooncontrol roon scenes scnews
  scoreboard screencast server smoke snowcrash stocks swimweek tantra test
  traffic videotest voice volumio waterfalls weather YouTube

or any other config file you have created in /usr/local/MagicMirror/config of the form:

 config-<name>.js

A config filename argument will be resolved into a config filename of the form:

 config-$argument.js

A subdirectory in which to locate the config file can be specified as the second argument, e.g. 'mirror foo bar' will attempt to use the config file bar/config-foo.js

The mirror command will attempt to match the specified config file name. For example, 'mirror foo' would match the config file named config-food.js

Arguments can also be specified as follows:

 -a <artist>, -A <artist>, -b <brightness>, -B, -c <config>, -d, -i <info>,
 -V, -N, -R (toggle video play, play next video, replay video),
 -H, -h (Hide video, Show video),
 -L, use landscape display mode and use landscape designed configs/pics,
 -I, -l <list>, -r <rotate>, -s <screen>, -S, -m <model>, -M <model>,
 -p <photographer>, -P <photographer>, -w <dir>, -W <dir>
 -X <screen number>, -Z, -u

Examples:

 mirror  # Invoked with no arguments the mirror command displays a command menu
 mirror list active  # lists active modules
 mirror list configs  # lists available configuration files
 mirror restart  # Restart MagicMirror
 mirror fractals  # Installs configuration file config-fractals.js and restarts MagicMirror
 mirror info  # Displays all MagicMirror system information
 mirror info screen  # Displays MagicMirror screen information
 mirror dev  # Restarts the mirror in developer mode
 mirror rotate left/right/normal/inverted/default  # rotates the screen left, right, inverted, normal, or restores all screens to their default rotation
 mirror screen on  #  Turns the Display ON
 mirror screen off  # Turns the Display OFF
 mirror screenshot  # Takes a screenshot of the MagicMirror
 mirror screen num  # Switches mirror display to screen num
 mirror status [all]  # Displays MagicMirror status, checks config syntax
 mirror update  # Updates MagicMirror installation
 mirror update modules  # Updates installed MagicMirror modules
 mirror getb  # Displays current MagicMirror brightness level
 mirror setb 150  # Sets MagicMirror brightness level to 150
 mirror vol 50  # Sets MagicMirror volume level to 50
 mirror wh foobar  # Creates and activates a slideshow config with pics in foobar
 mirror whrm foobar  # Deactivate and remove slideshow in foobar
 mirror -u  # Display this usage message
```

### Getting to the Command Line

Learn how to open a command line terminal window on various systems,
how to get started using the command line, and see some examples of why
the command line interface is so powerful by reading the MirrorCommand
wiki article
[Introduction to Using the Command Line](https://gitlab.com/doctorfree/MirrorCommand/-/wikis/Introduction-to-Using-the-Command-Line){:target="_blank"}{:rel="noopener noreferrer"}.

Note that some screen switching operations on systems with multiple monitors
may move the window focus from the command line terminal window to the
MagicMirror window. When this happens the mouse and keyboard input are no
longer being sent to the terminal window. To restore focus in the terminal
window, move the mouse cursor over the terminal window and click.

If your terminal window is obscured by the fullscreen MagicMirror window
and you wish to return to the terminal window, press &lt;F11&gt; to exit
MagicMirror fullscreen and click the mouse in the terminal window.

### Switching screens

On systems with more than one monitor it is possible to move the MagicMirror
window from one screen to another. Screens are numbered starting with '1' so
the primary screen display is screen number 1 and the secondary screen
is screen number 2. Naturally, this does not correspond to the internal
numbers assigned to screens which start with '0'. The wise geeks at
Doctorwhen's Bodacious Laboratory always start counting things at 1, not 0.
They thought you might also.

### Using the mirror command to switch screens

You can use the `mirror` command to switch screens. For example, to move
the MagicMirror window from the primary screen to the secondary screen,
execute the command `mirror screen 2`. To switch screens without knowing
which screen is number 1 and which is number 2, execute the command
`mirror screen switch` or `mirror switch`.

To start MagicMirror on a specified screen execute the command:

`mirror -X <screen_number> ...`

### Using voice commands to switch screens

The `MirrorCommand` package installs several `MMM-GoogleAssistant` voice
recipes. One of these, the `MirrorCommand.js` recipe, enables several voice
commands that can be used to control the MagicMirror. See the section
[MMM-GoogleAssistant integration](https://mirrorcommand.dev/integration#mmm-googleassistant-integration){:target="_blank"}{:rel="noopener noreferrer"}
to get started with MMM-GoogleAssistant setup and integration.

Once you have MMM-GoogleAssistant working and you can talk to your mirror
and get a response from Google Assistant, you can add the `MirrorCommand.js`
recipe to the MagicMirror `config.js` to issue MirrorCommand voice commands.

Among the MirrorCommand voice commands there are the following which control
screen switching:

Assuming your `MMM-GoogleAssistant` key phrase ('Model' config value) is "computer",

- Move MagicMirror to screen 1
  - Say `computer` and pause
  - Say `screen one` or `mirror screen one`
- Move MagicMirror to screen 2
  - Say `computer` and pause
  - Say `screen two` or `mirror screen two`
- Switch the MagicMirror screen
  - Say `computer` and pause
  - Say `screen switch` or `switch screens` or `mirror screen switch`
