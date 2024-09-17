===================================
Install Ubuntu on the LicheeRV Dock
===================================

The `LicheeRV board`_ can be used in different configurations. We currently only supply an image for the LicheeRV with Dock.


Supported images
================

* Ubuntu 24.04 (Kinetic Kudu) pre-installed server:

  - :download:`ubuntu-22.10-preinstalled-server-riscv64+licheerv.img.xz <https://old-releases.ubuntu.com/releases/22.10/ubuntu-22.10-preinstalled-server-riscv64+licheerv.img.xz>`


Using the pre-installed server image
====================================

#. Flash the pre-installed server image to a microSD card (see
   :doc:`/how-to/flash-images`)

#. Insert the microSD card into the board

#. Connect to the serial :term:`UART` console (see :doc:`/how-to/uart-console`).

#. Power on the board

#. Wait for an output line confirming that `cloud-init`_ has finished running;
   this service is responsible for generating SSH keys, and creating the
   default user:

   .. code-block:: text

        [  291.932176] cloud-init[1282]: Cloud-init v. 22.3.4-0ubuntu1 finished at Thu, 20 Oct 2022 08:25:11 +0000. Datasource DataSourceNoCloud [seed=/var/lib/cloud/seed/nocloud-net][dsmode=net].  Up 291.79 seconds


#. Login with the user *ubuntu* and the default password *ubuntu*; you will be
   asked to choose a new password


Using the live server image
===========================


WiFi Configuration
==================

Install WiFi driver (this step is only needed prior to kernel 6.7. It is not needed for Ubuntu 24.04)

    #. To enable WiFi the DKMS package licheerv-rtl8723ds-dkms is needed needed on lower kernel releases. If your LicheeRV is connected to the network via a USB Ethernet adapter, that is easy:
    
        .. code-block:: text

            $ sudo apt-get update
            $ sudo apt-get install licheerv-rtl8723ds-dkms

    #. If your LiccheRV is not connected to the network you have to download the licheerv-rtl8723ds-dkms_*.deb file from the https://launchpad.net/ubuntu/+source/licheerv-rtl8723ds-dkms page and write it to the SD card.

    #. Once you have booted the device the package can be installed with

        .. code-block:: text
            
            $ sudo dpkg -i licheerv-rtl8723ds-dkms_*.deb

        The installation process requires building a kernel module and takes at least an hour.

Configure Netplan

    #. Configure :term:`Netplan` to connect to your local network. Find below a simple example of a :term:`Netplan` configuration file that must be stored in ``/etc/netplan`` directory with the extension ``*.yaml``:
    
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

        The file should be owned by root and chmod 600.
    
    #. Either reboot or execute the following commands to load the kernel module and apply the configuration:

        .. code-block:: text

            $ sudo modprobe 8723ds
            $ sudo netplan apply
            $ sudo systemctl restart systemd-networkd.service

        You should now be connected to your wireless network.


Limitations
===========

* Microphone does not work


.. _LicheeRV board: https://wiki.sipeed.com/hardware/en/lichee/RV/Dock.html
