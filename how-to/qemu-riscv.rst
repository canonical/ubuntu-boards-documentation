===============================
Install Ubuntu on QEMU (RISC-V)
===============================

Prerequisites
=============

To boot a RISC-V virtual machine you will need the following packages
installed:

* ``opensbi`` -- OpenSBI implements the RISC-V SBI

* ``qemu-system-misc`` (from :lp-pkg:`qemu`) -- :term:`QEMU` is used to emulate
  a RISC-V machine

* ``u-boot-qemu`` (from :lp-pkg:`u-boot`) -- U-Boot is the firmware
  implementing the :term:`UEFI` :term:`API` and loads :term:`GRUB`

The packages can be installed with the following commands:

.. code-block:: text

    sudo apt update
    sudo apt install opensbi qemu-system-misc u-boot-qemu


Using the pre-installed server image
====================================

#. Download one of the supported images:

   .. ubuntu-images::
       :releases: jammy-
       :image-types: preinstalled-server
       :archs: riscv64
       :matches: (22\.04.*\+unmatched\.img|riscv64\.img)

   .. on jammy, use the +unmatched image for QEMU; later releases should use
      the unsuffixed images, hence the horrid regex above

#. Unpack the image:

   .. code-block:: text

       xz -dk ubuntu-24.04.1-preinstalled-server-riscv64.img.xz


#. Optionally, if you want a larger disk, you can expand the disk (the
   file-system will be automatically resized too):

   .. code-block:: text

       qemu-img resize -f raw ubuntu-24.04-preinstalled-server-riscv64.img +5G


#. Next use u-boot-qemu to boot the virtual machine. A working example with all
   the options is:

   .. code-block:: text

       qemu-system-riscv64 \
           -machine virt -nographic -m 2048 -smp 4 \
           -kernel /usr/lib/u-boot/qemu-riscv64_smode/uboot.elf \
           -device virtio-net-device,netdev=eth0 -netdev user,id=eth0 \
           -device virtio-rng-pci \
           -drive file=ubuntu-24.04-preinstalled-server-riscv64.img,format=raw,if=virtio

   The important options to use are:

   * QEMU's generic virtual platform is selected by ``-machine virt``

   * The first stage firmware booted by QEMU is OpenSBI. Before QEMU 7.0 this
     had to be specified by the ``-bios`` option. This option is not needed
     with QEMU 7.0 or higher. It cannot be used with KVM.

   * The second stage firmware U-Boot is loaded into memory via ``-kernel
     /usr/lib/u-boot/qemu-riscv64_smode/uboot.elf``

   One can use pass through networking, adjust memory (``-m``) and CPU counts
   (``-smp``) as needed.

#. Watch the serial console output and wait for cloud-init to complete. It will
   show a line with the text 'Cloud-init finished':

   .. code-block:: text

       [   68.346028] cloud-init[703]: Cloud-init v. 22.2-0ubuntu1~20.04.3 finished at Thu, 22 Sep 2022 11:35:28 +0000. Datasource DataSourceNoCloud [seed=/var/lib/cloud/seed/nocloud-net][dsmode=net].  Up 68.26 seconds

#. Login with the user *ubuntu* and the default password *ubuntu*; you will be
   asked to choose a new password

cloud-init integration
----------------------

The image provides a CIDATA partition as fallback data-source for `cloud-init`_.
It configures sudo user ubuntu with password ubuntu and uses DHCP to set up
networking. You will be asked to change the password on first login.

If you wish to customize the user password, networking information, or add SSH
keys, etc., please, mount the CIDATA partition, and adjust the meta-data and
user-data files as needed. In a cloud setup you can rename or delete the files
to ensure that only data provided via the network is used.


Using the live server image
===========================

Installing live server image
----------------------------

#. Download one of the supported images:

   .. ubuntu-images::
       :releases: jammy-
       :image-types: live-server
       :archs: riscv64

#. Unpack the disk image:

   .. code-block:: text

       gzip -d ubuntu-22.04.5-live-server-riscv64.img.gz

#. Create the disk image on which you will install Ubuntu; 16 GiB should be
   enough

   .. code-block:: text

       fallocate -l 16G disk

#. Start the installer with:

   .. code-block:: text

       qemu-system-riscv64 -machine virt -m 4G -smp cpus=2 -nographic \
           -kernel /usr/lib/u-boot/qemu-riscv64_smode/u-boot.bin \
           -netdev user,id=net0 \
           -device virtio-net-device,netdev=net0 \
           -drive file=ubuntu-22.04.5-live-server-riscv64.img,format=raw,if=virtio \
           -drive file=disk,format=raw,if=virtio \
           -device virtio-rng-pci

#. Follow the installation steps in
   `Ubuntu Server installation tutorial
   <https://ubuntu.com/tutorials/install-ubuntu-server>`_

When rebooting we have to remove the installer image. Otherwise the installer
will restart.

U-Boot gives you a 2 second time window to press the Enter key to reach the
U-Boot console. In U-Boot’s console you can use the poweroff command to stop
QEMU. Another option to exit QEMU is pressing keys ``CTRL-a`` followed by key
``x``.


Running Ubuntu
--------------

To run your installed Ubuntu image use:

.. code-block:: text

    qemu-system-riscv64 -machine virt -m 4G -smp cpus=2 -nographic \
        -kernel /usr/lib/u-boot/qemu-riscv64_smode/u-boot.bin \
        -netdev user,id=net0 \
        -device virtio-net-device,netdev=net0 \
        -drive file=disk,format=raw,if=virtio \
        -device virtio-rng-pci


Cloud-init seed
===============

Sample files for a cloud-init seed are present on the FAT partition labeled
"CIDATA". See :doc:`/how-to/headless-usage` for more information.


Limitations
===========

* The number of virtual CPUs was limited to 8 before QEMU 7.0. The limit was
  raised in QEMU 7.0 to 512.
