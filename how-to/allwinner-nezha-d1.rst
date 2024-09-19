========================================
Install Ubuntu on the Allwinner Nezha D1
========================================


Supported images
================

* Ubuntu 24.04 (Noble Numbat) pre-installed server:

  - :download:`ubuntu-24.04.1-preinstalled-server-riscv64+nezha.img.xz <https://cdimage.ubuntu.com/releases/24.04.1/release/ubuntu-24.04.1-preinstalled-server-riscv64+nezha.img.xz>`


Using the pre-installed server image
====================================

#. Flash the pre-installed server image to a microSD card (see
   :doc:`/how-to/flash-images`)

#. Insert the microSD card into the board

#. Connect to the serial :term:`UART` console. The serial console runs at 115200 baud (see :doc:`/how-to/uart-console`).

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


Limitations
===========

* Wifi/BT not working: the WiFi driver is only available on the vendor kernel

* Ethernet MAC address changes at every boot: this “feature” is described here along with the solutions to fix this in section “Setting the MAC address”

* Shutdown fails

.. _cloud-init: https://cloudinit.readthedocs.io/