---
title: Mirror Voice Commands with Google Assistant
author: doctorfree
date: 2023-07-10 16:20:00 +0800
tags: [voice, commands, magicmirror, google assistant, assistant, voice command]
pin: true
img_path: "/posts/20230710"
---

## Overview

Activating and configuring the
[MMM-GoogleAssistant](http://wiki.bugsounet.fr/en/MMM-GoogleAssistant)
and [MMM-Detector](http://wiki.bugsounet.fr/en/MMM-Detector)
modules on a [MagicMirror](https://magicmirror.builders/) can enable
the use of recipes to control MagicMirror with voice commands using Google Assistant.

Google Assistant voice control of MagicMirror utilizes a command line interface
that issues commands to the MagicMirror system. The shell command is executed by a
MagicMirror module without the need for SSH.

To utilize this method of MagicMirror voice control it is necessary to get a voice
assistant working then create voice triggers in that assistant that
activate a corresponding action which executes a MirrorCommand command.

I will describe in detail this method for voice control of MagicMirror
using Google Assistant and the setup precedure required.

## Table of Contents

1. [Google Assistant Requirements](#google-assistant-requirements)
1. [Google Assistant Setup](#google-assistant-setup)
1. [Google Assistant MagicMirror Control Setup](#google-assistant-magicmirror-control-setup)
1. [MirrorCommand Setup](#mirrorcommand-setup)
1. [MirrorCommand Installation](#mirrorcommand-installation)
    1. [Pre Installation](#pre-installation)
    1. [Debian Package installation](#debian-package-installation)
    1. [RPM Package installation](#rpm-package-installation)
    1. [ALSA audio input and output devices configuration](alsa-audio-input-and-output-devices-configuration)
1. [Post installation configuration](#post-installation-configuration)
1. [MagicMirror Configuration](#magicmirror-configuration)
1. [Activate MagicMirror Voice Control](#activate-magicmirror-voice-control)
1. [References](#references)

### Google Assistant Requirements

This method for voice control of MagicMirror requires a
[MagicMirror](https://magicmirror.builders/) with the
[MMM-GoogleAssistant](http://wiki.bugsounet.fr/en/MMM-GoogleAssistant)
and [MMM-Detector](http://wiki.bugsounet.fr/en/MMM-Detector) modules
activated and configured properly. Setup for this method is considerably
more difficult but once accomplished results in a superior quality setup
with far more ease and flexibility of use. The Siri setup requires SSH and
the Apple Shortcuts are more difficult to setup and maintain than the
Google Assistant voice recipes. However, Apple Shortcuts do offer advantages
over Google Assistant, especially in a noisy environment where voice commands
may be unreliable. I use both.

To get started with a MagicMirror installation, see the
[MagicMirror Documentation](https://docs.magicmirror.builders/).
Alternately, and preferably, install the
[MirrorCommand package](https://gitlab.com/doctorfree/MirrorCommand)
which performs an automated installation and configuration of MagicMirror
along with required modules.

### Google Assistant Setup

If you have a MagicMirror with a microphone then you can setup Google Assistant
by following the instructions at the
[MMM-GoogleAssistant wiki](http://wiki.bugsounet.fr/en/MMM-GoogleAssistant).
This setup is not simple as it requires a Google Project with Actions and OAuth
credentials. However, I found this 12 year old girl on YouTube who did it and
so I figured I could too. It turns out I could so you probably can too.

MMM-GoogleAssistant requires MMM-Detector which can also be setup and configured
following the instructins at the
[MMM-Detector wiki](http://wiki.bugsounet.fr/en/MMM-Detector).

### Google Assistant MagicMirror Control Setup
After activating and configuring MMM-GoogleAssistant and MMM-Detector it is now
time to tackle setting up MagicMirror control with Google Assistant. Test the initial
MMM-GoogleAssistant setup by saying something like "Hey Google" or "OK Google"
or "Computer" or whatever your MMM-Detector "Model" setting is. MMM-Detector
should wake up and listen. Then say something like "What time is it" and verify
that you get a response from Google Assistant. It it's working we can proceed.

### MirrorCommand Setup
Voice control of MagicMirror as described in this document requires the
use of a command line interface to issue the MagicMirror commands. This is
accomplished with the
[MirrorCommand](https://gitlab.com/doctorfree/MirrorCommand)
package which contains a shell command that acts as a frontend to
various MagicMirror and system functions.

See the
[MirrorCommand README](https://gitlab.com/doctorfree/MirrorCommand/-/blob/master/README.md)
for an overview of this package and documentation on its installation,
configuration, and use.

Many Google Assistant voice triggers are preconfigured as MMM-GoogleAssistant
recipes in the [MirrorCommand package](https://gitlab.com/doctorfree/MirrorCommand).

**NOTE:** The MirrorCommand installation will install MagicMirror and required
modules if you have not already installed MagicMirror. This automated installation
and configuration of MagicMirror and modules is the preferred installation procedure.

### MirrorCommand Installation

MirrorCommand v2.0.0 and later can be installed on Linux and Raspberry Pi
systems using either the Debian packaging format or the Red Hat Package
Manager (RPM).

#### Pre Installation

**XHOST:** The automated configuration requires access to some X11 graphical
utilities. Depending upon your system's X11 configuration, it may be necessary
to grant the *root* user access to the display. To do so, prior to installation
issue the command:

`xhost +si:localuser:root`

or grant everyone access with

`xhost +`

#### Debian Package installation

Many Linux and Raspberry Pi distributions, most notably Ubuntu and its derivatives,
use the Debian packaging system.

To tell if a system is Debian based it is usually sufficient to
check for the existence of the file `/etc/debian_version` and/or examine the
contents of the file `/etc/os-release`.

To install on a Debian based system, download the latest Debian format
package from the
[MirrorCommand Releases](https://gitlab.com/doctorfree/MirrorCommand/-/releases).

Install the MirrorCommand package by executing the command

```bash
sudo apt install ./MirrorCommand_<version>-<release>.deb
```
or
```console
sudo dpkg -i ./MirrorCommand_<version>-<release>.deb
```

#### RPM Package installation

Red Hat Linux, SUSE Linux, and their derivatives use the RPM packaging
format. RPM based Linux distributions include Fedora, AlmaLinux, CentOS,
openSUSE, OpenMandriva, Mandrake Linux, Red Hat Linux, and Oracle Linux.

To install on an RPM based Linux system, download the latest RPM format
package from the
[MirrorCommand Releases](https://gitlab.com/doctorfree/MirrorCommand/-/releases).

Install the MirrorCommand package by executing the command

```bash
sudo yum localinstall ./MirrorCommand_<version>-<release>.rpm
```
or
```console
sudo rpm -i ./MirrorCommand_<version>-<release>.rpm
```

#### ALSA audio input and output devices configuration
The MirrorCommand installation attempts to detect and configure ALSA
audio input and output devices such as a microphone, webcam, or DAC.

The installation process will modify `/etc/asound.conf` if it detects
audio devices incorrectly configured. No changes are made to individual
users' `.asoundrc` in their home directories. If you wish to override the
settings configured by the `set_asound_conf` command, you can do so by
creating an `.asoundrc` file in your home directory and managing the ALSA
audio device settings there.

To reconfigure the `/etc/asound.conf` ALSA audio configuration file,
issue the command:

`sudo /usr/local/bin/set_asound_conf`

### Post installation configuration

The MirrorCommand installation process cannot automatically configure
your private keys which are used to access various services the MagicMirror
utilizes. For example, you may have private keys to access a weather service,
Telegram, Google services, or the MMM-Remote-Control module.

Before you can use the MirrorCommand utilites and config files you will
need to add any keys you wish to use to the appropriate config files and utilities.

#### Add keys to mirrorkeys

**Don't Panic!** The MirrorCommand package includes utilities to add and
remove private keys. To do so:

Edit the file `/usr/local/MirrorCommand/etc/mirrorkeys` adding the keys you have
previously generated/retrieved to each of the 'keys[FOO]' settings with corresponding
'dumb[FOO]' setting, leaving the 'dumb[FOO]' setting as-is

Add the keys you wish to set and leave those you do not wish to set empty

After adding your keys, execute the command

```bash
  '/usr/local/MirrorCommand/bin/showkeys'
```

The `showkeys` command will read the `mirrorkeys` file and edit the appropriate
configuration files in `/usr/local/MirrorCommand` containing the placeholder dummy
settings.

For more info on the `showkeys` command and the
`/usr/local/MirrorCommand/etc/mirrorkeys` configuration file, see the
man pages
`[showkeys.1](https://gitlab.com/doctorfree/MirrorCommand/-/wikis/showkeys.1)`
and
`[mirrorkeys.5](https://gitlab.com/doctorfree/MirrorCommand/-/wikis/mirrorkeys.5)`
by executing the `man` command:

`man showkeys`

`man 5 mirrorkeys`

Unfortunately, it is not possible to automate this process any further than is
done here with the `showkeys` command and `mirrorkeys` configuration file.
There are nearly 40 preconfigured dummy key values and corresponding empty keys
settings in the distributed `/usr/local/MirrorCommand/etc/mirrorkeys` file.
It is a tedious task to acquire all these keys but that is the state of the
art in 21st Century Internet services at this time. On the plus side, all of
these services can be obtained without charge. Perhaps in the future some
enterprising young entrepreneur will create a meta-service that can generate
keys for the myriad of services available via the web.

It is strongly recommended that you take the time to acquire those keys you
will need to access the services your MagicMirror will be activating prior to
or immediate following installation of the MirrorCommand package. It is not
necessary to obtain keys for all of the services, only those you may use.
For example, you may intend to deploy a MagicMirror as a News, Weather,
and Stock tracking display. In that case, the only keys you may need to
acquire might be an OpenWeather API key, Dark Sky API key, IEX Cloud API key,
and CoinMarketCap API key. Leaving all other key settings blank in the
`mirrorkeys` file will not effect display of activated and configured
services - it simply does not enable access to those services you do not use.

#### Configure mirror script

Edit the main MagicMirror management script,
[**/usr/local/MirrorCommand/bin/mirror**](mirror.sh), setting:

- Location of your MagicMirror installation
- IP address of your MagicMirror
- Port for your MMM-Remote-Control module
- MMM-Remote-Control API Key (this is configured by `showkeys` above)
- Configuration subdirectories

Defaults for these are:

- MM="${HOME}/MagicMirror"
- IP="MM.M.M.MM"
- PORT="8080"
- apikey="xxx_Remote-Control-API-Key_xxxxx"
- CONF_SUBDIRS="Artists JAV Models Photographers"

In most cases you will only need to set the MMM-Remote-Control API key.
The IP setting should have been configured properly during installation and the
MMM-Remote-Control API key is set by the `showkeys` command after the `mirrorkeys`
file has been configured with the API key.

If you have not configured an API key for MagicMirror remote control then
set the apikey to blank ( <code>apikey=</code> ).

#### Rerun initialization scripts

The MirrorCommand installation process attempts to configure the audio and
video display settings of the system. These configuration scripts can be
rerun post-installation if reconfiguration is desired. For example, if the
installation was performed in the absence of a running X server then the
video display settings may be incorrect. Or, if the audio settings changed
due to the addition of a USB audio device after installation then the audio
settings may need to be re-initialized.

To perform these adjustments post-installation rerun the initialization scripts.

To adjust the video display settings, execute the command:

`/usr/local/MirrorCommand/etc/set_mirror_screens`

The `set_mirror_screens` command will prompt for the display mode, Portrait
or Landscape, and configure the file `/usr/local/MirrorCommand/etc/mirrorscreen`.
This command should be run when the display setup changes. For example, if
an additional monitor is added to the system or the existing monitor is upgraded
with a higher resolution or display mode.

To adjust the audio input/output settings, execute the command:

`sudo /usr/local/bin/set_asound_conf -e`

The `set_asound_conf` command will provide a dialog to select the desired audio
output and input devices and configure the file `/etc/asound.conf`. This
command should be run when the audio setup changes. For example, if an audio
USB device is added to the system or you wish to change configured audio
input/output devices. This command can also be used to check the current
configuration with `sudo set_asound_conf -c`, restore the original configuration
with `sudo set_asound_conf -r`, and select a configuration for you with
`sudo set_asound_conf -e -n`. See `set_asound_conf -u` for a full usage message.

### MagicMirror Configuration
MagicMirror with the MirrorCommand package uses config files in
`/usr/local/MagicMirror/config/` to control the behavior of the MagicMirror.
The MirrorCommand package installs many preconfigured MagicMirror config
files, many of which already have MMM-GoogleAssistant and MMM-Detector preconfigured.
One of these, `/usr/local/MagicMirror/config/config-default.js` can be used as
a guide and example of how to setup a MagicMirror config file for use with
Google Assistant voice control.

In `config-default.js` see the `recipes:` section and note the inclusion of the
`MirrorCommand.js` recipe. Include this Google Assistant recipe in your MagicMirror
config file to enable the preconfigured MagicMirror voice commands.

### Activate MagicMirror Voice Control
Once a MagicMirror config file has been created with the `MirrorCommand.js`
Google Assistant recipe included in the `recipes:` section of the
MMM-GoogleAssistant module, activate MagicMirror voice control via the command line.
Assuming the MagicMirror config file you wish to activate is
`/usr/local/MagicMirror/config/config-default.js`, execute the command:

`mirror default`

This should copy the `config-default.js` file into `config.js` and start
MagicMirror using this config file. If startup is successful the preconfigured
MagicMirror voice commands should be available. These voice commands include:

- "mirror restart"
- "mirror rotate inverted"
- "mirror rotate normal"
- "mirror rotate right"
- "mirror rotate left"
- "mirror screen off"
- "mirror screen on"
- "screen one|screen 1|mirror screen one|mirror screen 1"
- "screen two|screen 2|mirror screen two|mirror screen 2"
- "screen switch|switch screens|mirror screen switch|switch screen"
- "mirror stop"
- "mirror sound off"
- "mirror mute"
- "mirror sound on"
- "mirror unmute"
- "mirror art"
- "mirror cal"
- "mirror candy"
- "mirror crypto"
- "mirror default"
- "mirror fractals"
- "mirror gif"
- "mirror frame"
- "mirror nature"
- "mirror network"
- "mirror news"
- "mirror owls"
- "mirror portal"
- "mirror radar"
- "mirror rune"
- "mirror scenes"
- "mirror santa cruz"
- "mirror scores"
- "mirror smoke"
- "mirror snow crash"
- "mirror stocks"
- "mirror traffic"
- "mirror waterfalls"
- "mirror weather"
- "mirror youtube"

The MMM-Detector module is used to detect Google Assistant voice activation.
In the MMM-Detector configuration section of the MagicMirror `config.js` a
keyword or key phrase is defined. See the `Model:` setting(s) in the
MMM-Detector config. When this "Model" phrase is spoken it wakes up MMM-Detector
and the module listens for a command or query. For example, the default
`config-default.js` uses the Model keyword "computer" to instruct MMM-Detector
to listen for a command. Other "Model" settings use "ok google" or "hey google"
to wakeup MMM-Detector.

Test the MagicMirror Google Assistant by saying something like
"computer, what time is it" or "hey google, what is the weather". Pause briefly
between the keyword/keyphrase and the query/command. If this is not working,
a common problem is misconfiguratioin of the MagicMirror audio input and output
devices. ALSA audio input/ouput configuration can be performed by executing the
command `sudo /usr/local/bin/set_asound_conf -e`. After reconfiguring ALSA,
try the simple Google Assistant commands again.

Test voice control of MagicMirror with a simple command like "computer, mirror default".
Stop MagicMirror by saying "computer, mirror stop". If this is working try something a
little more difficult like "computer, mirror screen off" and "computer, mirror screen on".

### References
- [MagicMirror](https://magicmirror.builders/)
- [MMM-GoogleAssistant wiki](http://wiki.bugsounet.fr/en/MMM-GoogleAssistant)
- [MMM-Detector wiki](http://wiki.bugsounet.fr/en/MMM-Detector)
