============================================
Install Ubuntu on the DeepComputing FML13V01
============================================

The `DeepComputing FML13V01`_ is a RISC-V based motherboard for a
FrameWork 13" laptop.

.. note::

   The vendor provides an image using the Ubuntu userland, a vendor kernel, and
   a closed source graphics stack.

   The Ubuntu image with for StarFive JH7110 based boards neither supports the
   GPU nor PCIe.

Using the pre-installed server image
====================================

#. Download one of the supported images:

   * `Ubuntu 25.04 (Plucky Puffin) Server preinstalled <https://cdimage.ubuntu.com/releases/25.04/release/ubuntu-25.04-preinstalled-server-riscv64+jh7110.img.xz>`_

#. Flash the pre-installed server image to a microSD card (see
   :doc:`/how-to/flash-images`)

#. Insert the microSD card into the board

#. Set the boot source to the microSD card (see `Boot source selection`_)

#. Connect a USB UART adapter to the :term:`UART` on the :term:`GPIO` header
   (see `UART console`_ and :doc:`/how-to/uart-console`)

#. Power on the board

#. When "Hit any key to stop autoboot" is displayed, press :kbd:`Enter`

#. Reset the U-Boot environment with the following commands:

   .. code-block:: text

       env default -f -a
       env save

#. Power cycle the board

#. Wait for an output line confirming that `cloud-init`_ has finished running;
   this service is responsible for generating SSH keys, and creating the
   default user:

   .. code-block:: text

       [   35.682018] cloud-init[909]: Cloud-init v. 24.1.3-0ubuntu3 finished at Tue, 23 Apr 2024 07:44:59 +0000. Datasource DataSourceNoCloud [seed=/var/lib/cloud/seed/nocloud-net][dsmode=net].  Up 35.65 seconds

#. Login with the user *ubuntu* and the default password *ubuntu*; you will be
   asked to choose a new password


Using the live server image
===========================

The live installer image is used to install Ubuntu to an eMMC, USB, or NVMe
drive. To boot the live image, U-Boot must be installed on the SPI flash.

.. warning::

    The installer in Ubuntu 24.10 and 25.04 fails to invoke flash-kernel and
    grub-update. The system is booted with the device-tree from U-Boot and
    fails with an outdated U-Boot. Please, update U-Boot as described below.


Install U-Boot to the SPI flash
-------------------------------

#. Download one of the supported images:

   * `Ubuntu 25.04 (Plucky Puffin) Server live installer <https://cdimage.ubuntu.com/releases/25.04/release/ubuntu-25.04-live-server-riscv64.iso>`_

#. Flash the pre-installed server image to a microSD card (see
   :doc:`/how-to/flash-images`)

#. Insert the microSD card into the board

#. Set the boot source to the microSD card (see `Boot source selection`_)

#. Connect a USB UART adapter to the :term:`UART` on the :term:`GPIO` header
   (see `UART console`_ and :doc:`/how-to/uart-console`)

#. Power on the board

#. When "Hit any key to stop autoboot" is displayed, press :kbd:`Enter`

#. Enter the following commands to flash U-Boot to SPI:

   .. code-block:: text

       sf probe
       load mmc 1:1 $kernel_addr_r /usr/lib/u-boot/starfive_visionfive2/u-boot-spl.bin.normal.out
       sf update $kernel_addr_r 0 $filesize
       load mmc 1:1 $kernel_addr_r /usr/lib/u-boot/starfive_visionfive2/u-boot.itb
       sf update $kernel_addr_r 0x100000 $filesize

#. Switch the board off

#. Set the boot source to the SPI flash (see `Boot source selection`_)

#. Power on the board

#. When "Hit any key to stop autoboot" is displayed, press :kbd:`Enter`

#. Enter the following commands to reset the U-Boot environment:

   .. code-block:: text

       env default -f -a
       env save

#. Switch the board off


Boot the live server image
--------------------------

#. Flash the live server image to a microSD card (see
   :doc:`/how-to/flash-images`)

#. Insert the microSD card into the board

#. Ensure the boot source is SPI flash (see `Boot source selection`_), *not*
   microSD card

#. Connect a USB UART adapter to the UART on the GPIO header (see
   `UART console`_ and :doc:`/how-to/uart-console`)

#. Power on the board

#. If no prior operating system installation exists, U-Boot will start up GRUB
   on the microSD card. Go to step 8

#. If a prior operating system exists, press :kbd:`Enter` when "Hit any key to
   stop autoboot" is displayed, and enter:

   .. code-block:: text

       load mmc 1:1 $fdt_addr_r dtb/starfive/jh7110-deepcomputing-fml13v01.dtb
       load mmc 1:1 $kernel_addr_r EFI/boot/bootriscv64.efi
       bootefi $kernel_addr_r $fdt_addr_r

#. From the GRUB menu, select "Try or Install Ubuntu Server"

#. Loading the installer takes some time. Once it is loaded, follow the
   `Ubuntu Server installation tutorial
   <https://ubuntu.com/tutorials/install-ubuntu-server>`_

#. Once the installation is complete, reboot the board without removing the
   microSD card

.. note::

    U-Boot does not allow the operating system to write :term:`UEFI` variables.
    You can do this manually using U-Boot's :command:`eficonfig` command.


Boot source selection
=====================

The DeepComputing FML13V01 board can boot firmware from SPI flash, microSD card,
eMMC, or UART. The boot source is selected via DIP switches.

======  ======  ============
GPIO01  GPIO00  Boot Source
======  ======  ============
0       0       SPI flash
0       1       microSD card
1       0       eMMC
1       1       UART
======  ======  ============

For example, to boot from microSD card the DIP switches should be set as
follows:

.. image:: /images/deepcomputing-fml13v01-boot-source-sd.jpg
    :width: 15em
    :alt: Boot from microSD card

Alternatively, to boot from SPI flash, set the DIP switches like so:

.. image:: /images/deepcomputing-fml13v01-boot-source-spi.jpg
    :width: 15em
    :alt: Boot from SPI flash


Cloud-init seed
===============

Sample files for a cloud-init seed are present on the FAT partition labeled
"CIDATA". See :doc:`/how-to/headless-usage` for more information.


UART console
============

The :term:`UART` is available via the right lower USB-C connector and a
breakout board.

Assuming the typical coloring of USB to :term:`TTL` serial adapters the
following connections have to be made:

============== ==========
Breakout board Adapter
============== ==========
GND            GND, black
SBU1           RX,  white
SBU2           TX,  green
============== ==========

Do not connect the red 3.3 V wire.

SBU1 and SBU2 are swapped when turning the USB-C connector by 180°.

Connect with the following settings (see :doc:`/how-to/uart-console`):

* 115200 baud
* 8 data bits
* no parity
* 1 stop bit
* no flow control


Limitations
===========

* The on-board GPU is not supported

* PCIe support is incomplete: an NVMe drive can be used, but Wi-Fi cards and
  external GPUs don't work

.. _DeepComputing FML13V01: https://frame.work/de/en/products/deep-computing-risc-v-mainboard
