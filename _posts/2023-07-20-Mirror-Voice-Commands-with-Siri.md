---
title: Mirror Voice Commands with Siri
author: doctorfree
date: 2023-07-20 16:20:00 +0800
tags: [voice, commands, magicmirror, siri, assistant, voice command]
pin: true
img_path: "/posts/20230720"
---

## Overview

Siri voice control of MagicMirror utilizes a command line interface
that issues commands to the MagicMirror system. The shell command is
executed using an SSH Shortcut.

Apple SSH shortcuts can be used to execute commands on systems
that allow SSH access. On an iOS device create shortcuts which
use the “Run script over SSH” option for Apple Scripting shortcuts.
The shortcuts execute a Bash command which is a shell script that
executes the Python script with appropriate arguments.

I will describe in detail this method for voice control of MagicMirror
using Siri and the setup precedure required.

## Table of Contents

1. [MirrorCommand Setup](#mirrorcommand-setup)
1. [MirrorCommand Installation](#mirrorcommand-installation)
    1. [Debian Package installation](#debian-package-installation)
    1. [RPM Package installation](#rpm-package-installation)
    1. [ALSA audio input and output devices configuration](#alsa-audio-input-and-output-devices-configuration)
1. [Post installation configuration](#post-installation-configuration)
    1. [Add keys to mirrorkeys](#add-keys-to-mirrorkeys)
    1. [Configure mirror script](#configure-mirror-script)
    1. [Rerun initialization scripts](#rerun-initialization-scripts)
1. [MirrorCommand Test](#mirrorcommand-test)
1. [Apple Siri Setup](#apple-siri-setup)
1. [Apple Siri MagicMirror Control Setup](#apple-siri-magicmirror-control-setup)
    1. [Navigating the Shortcuts App](#navigating-the-shortcuts-app)
    1. [Configuring the Shortcut](#configuring-the-shortcut)
    1. [Naming the Shortcut](#naming-the-shortcut)
    1. [Testing the Shortcut](#testing-the-shortcut)
        1. [Adding the Shortcut to the Home Screen](#adding-the-shortcut-to-the-home-screen)
    1. [Adding More Shortcuts](#adding-more-shortcuts)
    1. [Troubleshooting Shortcuts](#troubleshooting-shortcuts)
        1. [Verify Command Line Execution](#verify-command-line-execution)
        1. [Verify SSH Authentication](#verify-ssh-authentication)
        1. [Verify SSH Enabled](#verify-ssh-enabled)
        1. [Modify SSH Command](#modify-ssh-command)
1. [References](#references)

## MirrorCommand Setup
Voice control of MagicMirror as described in this document requires the
use of a command line interface to issue the MagicMirror commands. This is
accomplished with the
[MirrorCommand](https://gitlab.com/doctorfree/MirrorCommand){:target="_blank"}{:rel="noopener noreferrer"}
package which contains a shell command that acts as a frontend to
MagicMirror and system actions.

See the
[MirrorCommand README](https://gitlab.com/doctorfree/MirrorCommand/-/blob/master/README.md){:target="_blank"}{:rel="noopener noreferrer"}
for an overview of this package and documentation on its installation,
configuration, and use.

### MirrorCommand Installation

MirrorCommand v2.0.0 and later can be installed on Linux systems using
either the Debian packaging format or the Red Hat Package Manager (RPM).

#### Debian Package installation

Many Linux distributions, most notably Ubuntu and its derivatives, use the
Debian packaging system.

To tell if a Linux system is Debian based it is usually sufficient to
check for the existence of the file `/etc/debian_version` and/or examine the
contents of the file `/etc/os-release`.

To install on a Debian based Linux system, download the latest Debian format
package from the
[MirrorCommand Releases](https://gitlab.com/doctorfree/MirrorCommand/-/releases){:target="_blank"}{:rel="noopener noreferrer"}.

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
[MirrorCommand Releases](https://gitlab.com/doctorfree/MirrorCommand/-/releases){:target="_blank"}{:rel="noopener noreferrer"}.

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
`[showkeys.1](https://gitlab.com/doctorfree/MirrorCommand/-/wikis/showkeys.1){:target="_blank"}{:rel="noopener noreferrer"}`
and
`[mirrorkeys.5](https://gitlab.com/doctorfree/MirrorCommand/-/wikis/mirrorkeys.5){:target="_blank"}{:rel="noopener noreferrer"}`
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
**/usr/local/MirrorCommand/bin/mirror**, setting:

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

### MirrorCommand Test
Once the initial installation and configuration of MirrorCommand on a system
in your local network is accomplished, test the system to verify it can
communicate successfully with the MagicMirror system. Try a simple MagicMirror
command like running the default MagicMirror config file:

`mirror default`

After verifying the MirrorCommand package is successfully communicating
with the MagicMirror system, proceed to the next section.

## Apple Siri Setup

To get started with Apple SSH shortcuts first enable SSH access on the system
where the [MirrorCommand](https://gitlab.com/doctorfree/MirrorCommand){:target="_blank"}{:rel="noopener noreferrer"}
package was installed. Follow one of the many guides for setting up SSH
with Apple Shortcuts. For example, these two guides should get you started:

- [Remote control your Mac with your iPhone and SSH Key Shortcuts](https://dougbeal.com/2019/11/02/remote-control-your-mac-with-your-iphone-and-ssh-key-shortcuts/){:target="_blank"}{:rel="noopener noreferrer"}
- [Setting up SSH for Shortcuts](https://www.thoughtasylum.com/2020/06/01/setting-up-ssh-for-shortcuts/){:target="_blank"}{:rel="noopener noreferrer"}

Verify that Apple Siri is working and responds to queries like "Hey Siri",
"What time is it?". Verify that SSH Shortcuts are working by creating a
shortcut that performs a simple command like `ping` as described in the
guides above. On recent versions of Apple's iOS the Shortcuts setting for
"Allow Running Scripts" is disabled. You may need to enable "Allow Running
Scripts" in Shortcuts by visiting `Settings -> Shortcuts` on your iOS device
and enabling "Allow Running Scripts".

Once Siri and SSH Shortcuts are confirmed to be working, proceed to the next section.

## Apple Siri MagicMirror Control Setup
After verifying that Apple Siri is working, SSH Shortcuts are enabled and working,
and MirrorCommand has been installed and configured on a system in the same local
area network as your MagicMirror system, it's now time to configure MagicMirror
control using Apple Siri.

### Navigating the Shortcuts App
Open the `Shortcuts` app on your iOS device. Tap `My Shortcuts` and
`All Shortcuts`. Tap the `+` symbol at the top right corner of the
app window to create a new shortcut. Tap `Add Action` then `Scripting`.
Scroll down the list of scripting actions to the `Shell` category. Tap
`Run Script Over SSH`.

### Configuring the Shortcut
This should add the "Run script over SSH" action to the new shortcut.
Tap `Show More` to view and modify the details of the scripting action.
Set the `Host` entry to the IP address of the system on which MirrorCommand
was installed. For example, 10.0.1.55. Set the `User` entry to the user on
the MirrorCommand system that you want to run the command as. For example,
my user's username is "pi".

Tap `SSH Key` for `Authentication`. In the text box that says `Script`
enter the command you wish to execute. To get started, try something simple
like `mirror default` which sets the MagicMirror config to config-default.js
and starts the MagicMirror. Now that the details of the scripting action have
been set, tap `Next` in the top right of the `New Shortcut` window.

**NOTE:** Some users have reported their SSH Shortcuts do not work unless
they are run in a login shell. To run a Shortcut in a login shell preface
the command you wish to run with `bash -l -c ...` and surround the command in
quotes. For example, to run the command `mirror default` in a login shell,
when configuring the Shortcut Script, use the command:

`bash -l -c "mirror default"`

### Naming the Shortcut
Enter the name of the Shortcut. By default, the name of the Shortcut will be
used as the phrase Siri will recognize to run this Shortcut. Choose a Shortcut
name that both reflects the action it performs and will be easily recognized
by Siri. Voice assistants can be picky. For example, using "MagicMirror" in your
Shortcut name might seem like a good idea since it is performing a MagicMirror action
but "MagicMirror" will be heard as "Magic Mirror" (two words) and Siri will try to
execute the Shortcut with "Magic Mirror" in its name rather than "MagicMirror".
For this first Shortcut that uses the default config, let's try the Shortcut name
"Mirror Default".

### Testing the Shortcut
Now we can test voice control of our first Shortcut. You should be able to run
this Shortcut by saying "Hey Siri, Mirror Default". Try it out. After a short
delay your MagicMirror system should display the default MagicMirror config.

#### Adding the Shortcut to the Home Screen
To test the Shortcut without Siri, add the Shortcut to the iOS device Home
Screen. This is an added benefit of using Shortcuts for voice control. In
addition to voice control they can be run from the device allowing remote
MagicMirror control even in a noisy environment where voice control isa
unavailable. Testing the Shortcut in this manner allows us to verify the
Shortcut works and isolate any problems encountered.

To add the Shortcut to the iOS device Home Screen, tap the three dots in the
upper right corner of the Shortcut then tap the three dots in the upper right
corner of the Shortcut configuration screen. This brings up the "Details"
configuration panel for the Shortcut where you can specify the Shortcut name.
Tap "Add to Home Screen" and "Add". An icon for this Shortcut will now be on
the iOS device Home Screen and the Shortcut can be run by tapping that icon.

Running the Shortcut from the Home Screen icon enables a test of the Shortcut
without using Siri as well as providing the convenience of another quick and
easy way to control MagicMirror remotely.

### Adding More Shortcuts
More complicated commands can be configured. Follow the same procedure as above
but replace `mirror default` with whatever MirrorCommand command you like.
For example, to configure a Shortcut that takes a screenshot of your MagicMirror,
add the following command in the `Script` text box:

`mirror screenshot`

Rather than creating a new Shortcut every time you want to add some new command
to your Shortcuts, it is possible to duplicate an existing Shortcut and then
modify it. We can use this to avoid having to enter the IP address, Username,
and Authentication type when creating a new Shortcut. For example, in the
Shortcuts app, press and hold the `Mirror Default` Shortcut we created earlier.
In the menu that pops up, tap "Duplicate". A new Shortcut should be created
named "Mirror Default 1". Tap the three dots (...) in the top right corner of
the Shortcut which should bring up the newly created Shortcut ready for us to
configure it.

First, rename the Shortcut to something describing the action you will add
and will be easily recognized by Siri. To rename the Shortcut, tap the
three dots (...) in the top right and replace the current name with a new name.
For example, replace "Mirror Default" with "Mirror Screenshot". Note that the
name of the Shortcut does not need to be exactly what the action is. It just
needs to be descriptive enough to let you know what it does and it needs to be
simple enough to allow Siri to recognize it distinctly and without confusion.
After renaming the Shortcut, Tap `Done`.

In the `Run script over SSH` scripting action, tap "Show More". All of the
settings should be correct and need not be modified. The only things we need
to modify are the Script text box command to execute and the name of the Shortcut.
Replace the Script text box command `mirror default` with the command to
take a MagicMirror screenshot. For example, replace `mirror default` with
`mirror screenshot`. Tap `Done` in the top right.

Now you can take a MagicMirror screenshot by saying "Hey Siri, Mirror Screenshot"
or whatever you named the Shortcut to be.

### Troubleshooting Shortcuts
If your Siri command failed to run the Shortcut or the Shortcut did not
result in the desired action, perform some troubleshooting.

#### Verify Command Line Execution
Start by verifying the command you entered in the Script text box works when
executed at the command line. For example, in a terminal window on the system
where the SSH commands are being executed and as the user you entered in the
Shortcut "User" setting, attempt to run the command at the command line prompt:

`mirror screenshot`

Attempt to run the exact same command you entered in the Shortcut Script text
box. If it does not succeed as expected then the issue is not in your Shortcut
or Siri but in the MagicMirror command line setup.

#### Verify SSH Authentication
If it succeeds then move on to verifying SSH authentication is configured
properly. In the Shortcut app, tap the three dots in the upper right
corner of the Shortcut you wish to troubleshoot. Tap "Show More" in the
scripting action. Verify that `Authentication` is set to `SSH Key`.
Just below `Authentication` you should see `SSH Key` on a line with
"ed25519 Key" or something similar. Tap "ed25519 Key". Tap
`Share Public Key` and copy it or air drop it or send it to yourself in
Messages. Verify that this public key is in the `~/.ssh/authorized_keys` file
on the system where MirrorCommand is installed in the home directory of the
user you are using to execute the SSH commands. In my example above this was
the system with IP address 10.0.1.55 and the user `pi`. In a terminal window,
logged in as that user on that system, change directory to `~/.ssh` and open
the file `authorized_keys` in a text editor. Verify the copied/air dropped public
key is in that file and, if not, add it to the file.

#### Verify SSH Enabled
The system on which the SSH command is executed (in the example above, 10.0.1.55)
must have SSH access enabled. On Linux this means that the `ssh` service is enabled
and active.

Verify that the SSH service is enabled and active. On Linux a command similar to:

`systemctl status sshd`

should display the status of the SSH service.

#### Modify SSH Command
Using SSH can sometimes be tricky. The expected runtime environment does not
always contain the same settings as a direct user login. It may be necessary
to modify the command you use to specify the full path to the command or to
run the command in a login shell.

For example, we used "mirror default" and "mirror screenshot" in the examples
above which assumes that the `mirror` command is in the user's execution PATH.
When run via SSH this may not be true. To avoid this error we can specify the
full path to the `mirror` command. Replace "mirror default" or "mirror screenshot"
with "/usr/local/bin/mirror default" or "/usr/local/bin/mirror screenshot".

To run a Shortcut in a login shell preface the command you wish to run with
`bash -l -c ...` and surround the command in quotes. For example, to run the
command `mirror default` in a login shell, when configuring the Shortcut Script,
use the command:

`bash -l -c "mirror default"`

## References
- [Remote control your Mac with your iPhone and SSH Key Shortcuts](https://dougbeal.com/2019/11/02/remote-control-your-mac-with-your-iphone-and-ssh-key-shortcuts/){:target="_blank"}{:rel="noopener noreferrer"}
- [Setting up SSH for Shortcuts](https://www.thoughtasylum.com/2020/06/01/setting-up-ssh-for-shortcuts/){:target="_blank"}{:rel="noopener noreferrer"}
