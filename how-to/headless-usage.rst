=====================================
Configure your board for headless use
=====================================

All Ubuntu pre-installed images, both for cloud and :term:`SBCs <SBC>` use
`cloud-init`_ for their first boot setup, including initial user creation. It
is possible to customize the first boot to configure a board for remote usage.
This includes securely importing all necessary credentials, and making it
possible to locate easily on the local network, removing the need to ever
attach a screen or keyboard to the board.

You will need the following:

* An Ubuntu One (Launchpad) or GitHub user account

* An SSH public/private key-pair (see :manpage:`ssh-keygen(1)` for more
  information)

* Your SSH public key registered with Launchpad or GitHub


Cloud-init seeds
================

The cloud-init "seed" provides the configuration that cloud-init will act upon
during the first boot. The location of the seed is image dependent. On the
Ubuntu for Raspberry Pi images, it is present on the boot partition (the first
:term:`FAT` partition on the images). On other images, it may be present on a
:term:`FAT` partition labelled "CIDATA". The location will be documented in the
board's corresponding chapter in these docs.

Wherever it resides, the cloud-init seed always consists of the following
files, all in `YAML`_ format:

meta-data
    While mandatory, this is largely uninteresting for our purposes here. It
    simply "identifies" the board and specifies whether the seed has a
    network-based component (this is more useful in a cloud context)

user-data
    This defines the bulk of the cloud-init configuration, specifying initial
    user characteristics, packages to install, files to manipulate, and so on

network-config
    Optional to the seed, but required in our case to ensure that cloud-init
    has a valid network connection from which to retrieve keys and packages


Network configuration
---------------------

The :file:`network-config` file contains the :term:`netplan` compatible
configuration for networking cards present on the board.

.. Mandatory when accessing the network for anything in cloud-init

.. But can be WiFi or Ethernet (no specific restrictions on most boards)


Customising the default user
----------------------------

.. Setting the username and password (and password expiry)

.. Specifying an SSH key-set to import


Installing additional software
------------------------------

.. 


Optimisations
-------------

.. apt-cache configuration

.. _YAML: https://en.wikipedia.org/wiki/YAML
