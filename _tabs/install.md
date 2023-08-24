---
layout: post
icon: fas fa-arrow-circle-down
order: 1
toc: true
post_style: page
---

## MirrorCommand Installation

### MagicMirror Installation

MirrorCommand is intended for installation on a system running
[MagicMirror](https://magicmirror.builders){:target="_blank"}{:rel="noopener noreferrer"}. If MagicMirror is not
previously installed it can be installed following
[these instructions](https://docs.magicmirror.builders/getting-started/installation.html#manual-installation){:target="_blank"}{:rel="noopener noreferrer"}. Alternatively, the MirrorCommand Debian or RPM package
format installation will automatically install and configure MagicMirror if no
existing MagicMirror installation is detected. This automated installation
of MagicMirror includes installing and configuring PM2 for easy and powerful
process management of MagicMirror as well as the installation of several
MagicMirror 3rd party modules used by MirrorCommand.

**Note:** An actual mirror with display mounted on the rear is not necessary.
MagicMirror can be installed on a Raspberry Pi with a mirror display as intended or
it can be installed on a Debian based Linux system (e.g. Ubuntu Linux) or an RPM
based Linux system (e.g. Fedora Linux) with standard monitor and desktop environment.

MagicMirror requires Node version 12 or later.

MirrorCommand assumes that MagicMirror is installed in either
`/usr/local/MagicMirror` or a user's home directory.

To install MagicMirror simply install MirrorCommand using the Debian or RPM
package format available at the
[MirrorCommand Releases](https://gitlab.com/doctorfree/MirrorCommand/-/releases){:target="_blank"}{:rel="noopener noreferrer"}
area of the Git repository.

If MagicMirror is not installed in either `/usr/local/MagicMirror` or a non-root
user's home directory, then the MirrorCommand installation will ask if you
wish to install MagicMirror and, if so, will install MagicMirror in `/usr/local`.
In addition to performing a MagicMirror installation and configuration, the
MirrorCommand installation will install and configure several MagicMirror
modules and PM2 process management.

Alternatively, MagicMirror can be installed manually following these steps:

- Change directory to either `/usr/local` or a non-root user's home directory
- From either `/usr/local` or a non-root user's home directory, execute the following commands:
  - Download and install the latest Node.js version
    - `curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -`
    - `sudo apt install -y nodejs`
  - Clone the repository and check out the master branch
    - `git clone https://github.com/MichMich/MagicMirror`
  - Enter the repository
    - `cd MagicMirror`
  - Install the application
    - `npm install`
  - Make a copy of the config sample file
    - `cp config/config.js.sample config/config.js`
  - Start the application
    - `npm run start`

To start the MagicMirror over SSH from a remote terminal, use the commands:

- `cd /path/to/MagicMirror`
- `DISPLAY=${DISPLAY:=:0} nohup npm start &`

To access the toolbar menu when in mirror mode, press the ALT key.

### MirrorCommand Installation

MirrorCommand v3.0 and later can be installed on Linux systems using
either the Debian packaging format or the Red Hat Package Manager (RPM).

### Pre Installation

**XHOST:** The automated configuration requires access to some X11 graphical
utilities. Depending upon your system's X11 configuration, it may be necessary
to grant the _root_ user access to the display. To do so, prior to installation
issue the command:

`xhost +si:localuser:root`

or grant everyone access with

`xhost +`

**ROON:** If you plan to install both the
[RoonCommandLine package](https://gitlab.com/doctorfree/RoonCommandLine){:target="_blank"}{:rel="noopener noreferrer"}
and the
[MirrorCommand package](https://gitlab.com/doctorfree/MirrorCommand){:target="_blank"}{:rel="noopener noreferrer"}
on the same system then in order to enable automatic configuration of
MirrorCommand's Roon configuration files, the RoonCommandLine
package must be installed before the MirrorCommand package.

After granting X11 access with `xhost` and optionally installing
RoonCommandLine, install MirrorCommand following either the
[Debian package installation guide](#debian-package-installation)
or [RPM package installation guide](#rpm-package-installation).

### Debian Package installation

Many Linux distributions, most notably Ubuntu and its derivatives, use the
Debian packaging system.

To tell if a Linux system is Debian based it is usually sufficient to
check for the existence of the file `/etc/debian_version` and/or examine the
contents of the file `/etc/os-release`.

To install on a Debian based Linux system, download the latest Debian
format package from the
[MirrorCommand releases](https://gitlab.com/doctorfree/MirrorCommand/-/releases){:target="_blank"}{:rel="noopener noreferrer"}.

Install the MirrorCommand package by executing the command

```bash
sudo apt install ./MirrorCommand_<version>-<release>.deb
```

or

```console
sudo dpkg -i ./MirrorCommand_<version>-<release>.deb
```

### RPM Package installation

Red Hat Linux, SUSE Linux, and their derivatives use the RPM packaging
format. RPM based Linux distributions include Fedora, AlmaLinux, CentOS,
openSUSE, OpenMandriva, Mandrake Linux, Red Hat Linux, and Oracle Linux.

To install on an RPM based Linux system, download the latest RPM format
package from the
[MirrorCommand releases](https://gitlab.com/doctorfree/MirrorCommand/-/releases){:target="_blank"}{:rel="noopener noreferrer"}.

Install the MirrorCommand package by executing the command

```bash
sudo yum localinstall ./MirrorCommand_<version>-<release>.rpm
```

or

```console
sudo rpm -i ./MirrorCommand_<version>-<release>.rpm
```
