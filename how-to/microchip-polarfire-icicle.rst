================================================
Install Ubuntu on the Microchip Polarfire Icicle
================================================

FPGA Design
===========

Ubuntu's kernel 5.19 requires the `Icicle Kit Reference Design <https://github.com/polarfire-soc/icicle-kit-reference-design/releases>`_ v2022.10 or later. The archive "MPFS_ICICLE_BASE_DESIGN_yyyy_mm.zip" contains a job file and digest file which can be used. This job file also includes the latest version of the Hart Software Services at the time of the release (see below). `FlashPro Express <https://www.microchip.com/en-us/products/fpgas-and-plds/fpga-and-soc-design-tools/programming-and-debug/flashpro-and-flashpro-express>`_ is needed to program the job file to the FPGA. `FlashPro Express <https://www.microchip.com/en-us/products/fpgas-and-plds/fpga-and-soc-design-tools/programming-and-debug/flashpro-and-flashpro-express>`_ standalone is bundled with the `Programming and Debug Tools <https://www.microchip.com/en-us/products/fpgas-and-plds/fpga-and-soc-design-tools/programming-and-debug>`_.

Hart Software Services
======================

The first stage boot loader of the `PolarFire <https://www.microchip.com/en-us/products/fpgas-and-plds/fpgas/polarfire-fpgas>`_ SoC FPGA Icicle Kit is called `Hart Software Services (HSS) <https://github.com/polarfire-soc/hart-software-services/releases>`_. Ubuntu's kernel 5.19 requires version v2022.10 or later of this software. Microchip provides a `FlashPro <https://www.microchip.com/en-us/products/fpgas-and-plds/fpga-and-soc-design-tools/programming-and-debug/flashpro>`_ Express programming job that can be used to update the HSS on a kit to the latest version without having to build the software, this job is provided with the FPGA design. Otherwise the HSS sources are available on `GitHub <https://github.com/polarfire-soc/hart-software-services>`_ and can be built and programmed independently of the design using the command line or the `SoftConsole IDE <https://www.microchip.com/en-us/products/fpgas-and-plds/fpga-and-soc-design-tools/soc-fpga/softconsole>`_.


Supported images
================

* Ubuntu 24.04 (Noble Numbat) pre-installed server:

  - :download:`ubuntu-24.04.1-preinstalled-server-riscv64+icicle.img.xz <https://cdimage.ubuntu.com/releases/24.04.1/release/ubuntu-24.04.1-preinstalled-server-riscv64+icicle.img.xz>`

Using the pre-installed server image
====================================

#. Flash the pre-installed server image to a microSD card or the Icicle Kit on board eMMC (see
   :doc:`/how-to/flash-images`). The board uses package u-boot-microchip as second stage boot loader. It is installed in the partition with label loader on the above pre-installed server disk image. Ubuntu's U-Boot has been set up to use all U54 cores for Linux. If you want to use one or more cores for a real time operating system in parallel to Ubuntu, you will have to uninstall this package and install your own U-Boot onto partition loader.

#. Insert the microSD card into the board

#. Connect to the :term:`UART` console. If the Icicle board is the only connected USB UART, /dev/ttyUSB0 will show the HSS output and /dev/ttyUSB1 will be the U-Boot and Linux console (see `UART console`_ and :doc:`/how-to/uart-console`).

#. Power on the board

#. Wait for an output line confirming that `cloud-init`_ has finished running;
   this service is responsible for generating SSH keys, and creating the
   default user:

   .. code-block:: text

        [  291.932176] cloud-init[1282]: Cloud-init v. 22.3.4-0ubuntu1 finished at Thu, 20 Oct 2022 08:25:11 +0000. Datasource DataSourceNoCloud [seed=/var/lib/cloud/seed/nocloud-net][dsmode=net].  Up 291.79 seconds


#. Login with the user *ubuntu* and the default password *ubuntu*; you will be asked to choose a new password

Using the live server image
===========================


UART console
============

The board features a USB to quad :term:`UART` bridge controller (J11). The first UART has the HSS output, while the second :term:`UART` is used for the U-Boot and the Linux console. The baudrate is 115200.

USB
===

The USB OTG port (J16) can both be used a USB client or a USB host port. Jumpers on the board switch between client and host mode (J15) and control the provision of 5 Volt to the USB port (J17). The Ubuntu kernel supports the host mode.

Limitations
===========

* As of Linux kernel version 6.2, PCIe is not supported.
