=========================================
Install Ubuntu on the StarFive VisionFive
=========================================


Supported images
================

* Ubuntu 22.04 (Jammy Jellyfish) pre-installed server:

  - :download:`ubuntu-22.04.2-preinstalled-server-riscv64+visionfive.img.xz <https://old-releases.ubuntu.com/releases/22.04/ubuntu-22.04.2-preinstalled-server-riscv64+visionfive.img.xz>`


Using the pre-installed server image
====================================

#. Flash the pre-installed server image to a microSD card (see
   :doc:`/how-to/flash-images`)

#. Insert the microSD card into the board

#. Connect to the serial :term:`UART` console. Early boot output is only available on the serial console, which runs at 115200 baud (see :doc:`/how-to/uart-console`).

#. Power on the board

#. Wait for an output line confirming that `cloud-init`_ has finished running;
   this service is responsible for generating SSH keys, and creating the
   default user:

   .. code-block:: text

       [  150.053340] cloud-init[1274]: Cloud-init v. 23.1.1-0ubuntu2 finished at Mon, 03 Apr 2023 11:14:35 +0000. Datasource DataSourceNoCloud [seed=/var/lib/cloud/seed/nocloud-net][dsmode=net].  Up 150.00 seconds

#. Login with the user *ubuntu* and the default password *ubuntu*; you will be
   asked to choose a new password


Using the live server image
===========================


Wireless LAN
============

To use the wireless LAN you have to configure :term:`netplan`. Find below a simple example of a :term:`Netplan` configuration file that must be stored in ``/etc/netplan`` directory with the extension ``*.yaml``:

.. code-block:: text

    network:
    version: 2
    renderer: networkd
    wifis:
        wlan0:
        dhcp4: yes
        dhcp6: yes
        access-points:
            "YOUR_SSID":
            password: "YOUR_PASSWORD"

Either reboot or execute the following commands to apply the configuration:


.. code-block:: text

    sudo netplan apply
    sudo systemctl restart systemd-networkd.service


Bluetooth
=========

The firmware required to use Bluetooth is only available in a ppa:

#. Add the RISC-V PPA:

    .. code-block:: text

        sudo add-apt-repository ppa:ubuntu-risc-v-team/release

#. Install the required packages:

    .. code-block:: text

        sudo apt install linux-firmware-starfive brcm-patchram-plus-starfive


Limitations
===========

.. _cloud-init: https://cloudinit.readthedocs.io/