# A Pared Down Version of APT Pinning for Chromium

Don't want to install Chromium via `snapd`? This guide describes how to install Chromium from the Debian Buster repositories.

Do this:

Create and edit a .list file, and name it what you want in the `/etc/apt/sources.list.d` directory. I've chosen debian-chromium for easy searching.

```
sudo nano /etc/apt/sources.list.d/debian-chromium.list
```

Copy and paste the following lines into the .list file.

```
deb https://deb.debian.org/debian buster main
deb https://deb.debian.org/debian buster-updates main
deb http://security.debian.org/ buster/updates main
```

Create and edit a .pref file, and name it what you want in the `/etc/apt/preferences.d` directory. I've chosen debian-chromium again.

```
sudo nano /etc/apt/preferences.d/debian-chromium.pref
```

Copy and paste the following lines into the .pref file.

```
# Don't install anything other than chromium from the Debian repos
Package: *
Pin: origin "deb.debian.org"
Pin-Priority: -10

# Don't install anything other than chromium from the Debian repos
Package: *
Pin: origin "security.debian.org"
Pin-Priority: -10

# Exclude the game chromium-bsu
Package: chromium-bsu*
Pin: origin "deb.debian.org"
Pin-Priority: -10

# Exclude the game chromium-bsu
Package: chromium-bsu*
Pin: origin "security.debian.org"
Pin-Priority: -10

# Pattern includes 'chromium'
Package: chromium*
Pin: origin "deb.debian.org"
Pin-Priority: 700

# Pattern includes 'chromium'
Package: chromium*
Pin: origin "security.debian.org"
Pin-Priority: 700

# Chromium dependencies only in buster
Package: /libevent-2.1-6/ /libicu63/ /libjpeg62-turbo/ /libvpx5/
Pin: origin "deb.debian.org"
Pin-Priority: 1

# Chromium dependencies only in buster
Package: /libevent-2.1-6/ /libicu63/ /libjpeg62-turbo/ /libvpx5/
Pin: origin "security.debian.org"
Pin-Priority: 1
```

_The pin priorties are `-10` (do not install ever), `700` (install packages from this repo unless there is another from your main repo) and `1` (install this package over any other option)_

Then run each of the following commands individually. These commands pull the Debian signing keys from keyserver.ubuntu.com site so the things you get from the Debian repositories can be verified as legitimate. There are three key additions. Do all three.

```
sudo apt-key adv --keyserver hkps://keyserver.ubuntu.com:443 --recv-keys DCC9EFBF77E11517
```
```
sudo apt-key adv --keyserver hkps://keyserver.ubuntu.com:443 --recv-keys 648ACFD622F3D138
```
```
sudo apt-key adv --keyserver hkps://keyserver.ubuntu.com:443 --recv-keys 112695A0E562B32A
```

That was the heavy lifting. Now, update your system's package listing.

```
sudo apt update
```

Then install Chromium from Debian's repositories!

```
sudo apt install chromium
```

This method will keep you as up-to-date as users on Debian 10 and will make upgrading as easy as any other bit of software on the system through your distribution's software updater.

Enjoy.

## Sources

#### Original AskUbuntu Answer
https://askubuntu.com/a/1206153

#### Formalized for Linux Mint
https://linuxmint-user-guide.readthedocs.io/en/latest/chromium.html