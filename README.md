IMAGE FOR GERK ONLY DO NOT USE

## How to install:

Note: If you have an Nvidia GPU use [the ublue-os/nvidia images instead](https://github.com/ublue-os/nvidia)

To rebase an existing Silverblue/Kinoite machine to the latest release (37): 

1. Download and install [Fedora Silverblue](https://silverblue.fedoraproject.org/download)
1. After you reboot you should [pin the working deployment](https://docs.fedoraproject.org/en-US/fedora-silverblue/faq/#_about_using_silverblue) so you can safely rollback 
1. Open a terminal and use one of the following commands to rebase the OS:


**Silverblue (GNOME):**

    sudo rpm-ostree rebase ostree-unverified-registry:ghcr.io/liberaltugboat/silverblue-gblue

**Kinoite (KDE)**

    sudo rpm-ostree rebase ostree-unverified-registry:ghcr.io/liberaltugboat/kinoite-gblue
    
**LXQt**

    sudo rpm-ostree rebase ostree-unverified-registry:ghcr.io/liberaltugboat/lxqt-gblue
    
**MATE**

    sudo rpm-ostree rebase ostree-unverified-registry:ghcr.io/liberaltugboat/mate-gblue
    

**Vauxite (XFCE)**
    
    sudo rpm-ostree rebase ostree-unverified-registry:ghcr.io/liberaltugboat/vauxite-gblue


# Main

[![build-ublue](https://github.com/ublue-os/main/actions/workflows/build.yml/badge.svg)](https://github.com/ublue-os/main/actions/workflows/build.yml)

A WIP common main image for all other Ublue images.

1. [Features](#Features)
1. [Tips and tricks](#Tips and tricks)
1. [How to install](#How to install)
1. [Verification](#Verification)
1. [Configuring Automatic Updates](#Configuring Automatic Updates)
1. [Making your own](#Making your own)

## What is this?

You should be familiar with [immutable desktops](https://silverblue.fedoraproject.org/about). These are Fedora-ostree images that have been modified with the following quality of life features: 

## Features

- Start with a Fedora image
- Adds the following packages to the base image:
  - Hardware acceleration and codecs
  - `distrobox` for terminal CLI and user package installation
  - A selection of [udev rules and service units](https://github.com/ublue-os/config)
  - Various other tools: check out the [complete list of packages](packages.json)
- Sets automatic staging of updates for the system
- Sets flatpaks to update twice a day
- Everything else (desktop, artwork, etc) remains stock so you can use this as a good starting image

## Tips and Tricks

These images are immutable, you can't, and really shouldn't, install packages like in a mutable "normal" distribution.
Applications should be installed using Flatpak whenever possible (execpt for IDEs in some cases, more below).
Should that not be possible, you can use [distrobox](https://github.com/89luca89/distrobox) to have images of mutable distributions where you can install applications normally.
Want an application that is only available on Arch Linux *and* one that is only on Ubuntu? Well, now can have both!

Distrobox is very powerful, for example you can use to [host your entire development environment](https://github.com/89luca89/distrobox/blob/main/docs/posts/integrate_vscode_distrobox.md) completely separate from your host system. Or use it to run a [container for your virtual machines](https://github.com/89luca89/distrobox/blob/main/docs/posts/run_libvirt_in_distrobox.md).

ublue-os/base-main is also very well suited for servers, and users are expected to make full use of `podman` to host containers running "typical" server software i.e. `nginx`, `caddy` and others. 


These images are signed with sisgstore's [cosign](https://docs.sigstore.dev/cosign/overview/). You can verify the signature by downloading the `cosign.pub` key from this repo and running the following command:

    cosign verify --key cosign.pub ghcr.io/liberaltugboat/base-gblue
    

## Configuring Automatic Updates

> **Warning**
> 
> Disabling automatic updates is an unsupported configuration. If you reconfigure updates, you MUST be on the latest image before opening any issues.

With that said, you can individually disable which automatic update timers [ublue-os/config](https://github.com/ublue-os/config) provides with the following commands:

* flatpak system: `sudo systemctl disable flatpak-system-update.timer`
* flatpak user: `sudo systemctl --global disable flatpak-user-update.timer`

You can also configure automatic `rpm-ostree` updates by editing `/etc/rpm-ostreed.conf` and changing "AutomaticUpdatePolicy" to "none" or "check":

```
[Daemon]
AutomaticUpdatePolicy=check
```

## Making your own

See [the documentation](https://ublue.it/making-your-own/) on how use this image in your own projects.

### Architecture

This image can be used as an end user desktop or as something to derive from.
[The architecture](https://ublue.it/architecture/) looks like this:

### Adding Applications

Edit the `packages.json` file with your preferred applications.
Flatpak installation is a WIP.

