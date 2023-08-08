---
layout: post
icon: fas fa-info-circle
order: 2
toc: true
post_style: page
---

## Post installation configuration

### ALSA audio input and output devices configuration

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

### Add keys to mirrorkeys

The MirrorCommand installation process cannot automatically configure
your private keys which are used to access various services the MagicMirror
utilizes. For example, you may have private keys to access a weather service,
Telegram, Google services, or the MMM-Remote-Control module.

Before you can use the MirrorCommand utilites and config files you will
need to add any keys you wish to use to the appropriate config files and utilities.

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

### Configure mirror script

Edit the main MagicMirror management script,
[**/usr/local/MirrorCommand/bin/mirror**](https://github.com/doctorfree/MirrorCommand/blob/master/mirror.sh),
setting:

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

### Rerun initialization scripts

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

### Image archive installation

See the [MirrorImages repository](https://gitlab.com/doctorfree/MirrorImages)
to download several packages that can be used to download image archives
preconfigured for use with the MirrorCommand config files. This is optional
and is provided simply as a convenience. Note that none of the MirrorImages
packages actually contain images. These packages know how to download
image archives when they are installed on a system and know where to
extract these archives so the MirrorCommand config files can access them.
Some MirrorImages archive downloads can consume significant disk space.
Make sure you have sufficient available disk space before installing.
