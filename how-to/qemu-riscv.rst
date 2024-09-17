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

  sudo apt-get update
  sudo apt-get install opensbi qemu-system-misc u-boot-qemu


Supported images
================

* Ubuntu 24.04 (Noble Numbat) pre-installed server:

  - :download:`ubuntu-24.04.1-preinstalled-server-riscv64.img.xz <https://cdimage.ubuntu.com/releases/24.04/release/ubuntu-24.04.1-preinstalled-server-riscv64.img.xz>`

* Ubuntu 24.04 (Noble Numbat) live server:

  - :download:`ubuntu-24.04.1-live-server-riscv64.img.gz <https://cdimage.ubuntu.com/releases/24.04/release/ubuntu-24.04.1-live-server-riscv64.img.gz>`

* Ubuntu 22.04 (Jammy Jellyfish) live server:

  - :download:`ubuntu-22.04.5-live-server-riscv64.img.gz <https://cdimage.ubuntu.com/releases/22.04.3/release/ubuntu-22.04.5-live-server-riscv64.img.gz>`

Daily provided live server images are available at `https://cdimage.ubuntu.com/releases/24.04/release/ubuntu-24.04.1-live-server-riscv64.img.gz <https://cdimage.ubuntu.com/releases/24.04/release/ubuntu-24.04.1-live-server-riscv64.img.gz>`_.

Using the pre-installed server image
====================================

1. Unpack the image:

  .. code-block:: text
    
    xz -dk ubuntu-24.04.1-preinstalled-server-riscv64.img.xz
  
  (Up to Ubuntu 22.04. use the image for the SiFive HiFive Unmatched board, e.g. `ubuntu-22.04.3-preinstalled-server-riscv64+unmatched.img <https://old-releases.ubuntu.com/releases/22.04/ubuntu-22.04.3-preinstalled-server-riscv64+unmatched.img.xz>`_.)


2. Optionally, if you want a larger disk, you can expand the disk (the filesystem will be automatically resized too):

  .. code-block:: text

    qemu-img resize -f raw ubuntu-24.04-preinstalled-server-riscv64.img +5G


3. Next use u-boot-qemu to boot the virtual machine. A working example with all the options is:

  .. code-block:: text

    qemu-system-riscv64 \
    -machine virt -nographic -m 2048 -smp 4 \
    -bios /usr/lib/riscv64-linux-gnu/opensbi/generic/fw_jump.bin \
    -kernel /usr/lib/u-boot/qemu-riscv64_smode/uboot.elf \
    -device virtio-net-device,netdev=eth0 -netdev user,id=eth0 \
    -device virtio-rng-pci \
    -drive file=ubuntu-24.04-preinstalled-server-riscv64.img,format=raw,if=virtio

  The important options to use are:

    #. QEMU's generic virtual platform is selected by ``-machine virt``
    #. The first stage firmware booted by QEMU is OpenSBI as specified by the ``-bios`` option. This option is not needed with QEMU 7.0 or higher. It cannot be used with KVM.
    #. The second stage firmware U-Boot is loaded into memory via ``-kernel /usr/lib/u-boot/qemu-riscv64_smode/uboot.elf``

  One can use pass through networking, adjust memory (-m) & cpu counts (-smp) as needed.

4. Watch the serial console output and wait for cloud-init to complete. It will show a line with the text 'Cloud-init finished' like:

  .. code-block:: text
    
    [   68.346028] cloud-init[703]: Cloud-init v. 22.2-0ubuntu1~20.04.3 finished at Thu, 22 Sep 2022 11:35:28 +0000. Datasource DataSourceNoCloud [seed=/var/lib/cloud/seed/nocloud-net][dsmode=net].  Up 68.26 seconds

5. Login using ubuntu:ubuntu. See the cloud-init section below to further customize the first boot behavior with cloud-init.

cloud-init integration
----------------------

The image has a fallback cloud-init datasource that configures sudo user ubuntu with password ubuntu and DHCP networking. You will be asked to change the password on first login.

The image has CIDATA partition which can be used as a valid datasource to adjust cloud-config metadata. If you wish to customize user password, networking information, add ssh keys, etc. Please mount CIDATA partition rename meta-data and user-data files and adjust them to taste. You can use network-info to configure networking if something more sophisticated than just DHCP is desired.

For example ssh keys, disabling interactive login, and so on. See `https://cloudinit.readthedocs.io/en/latest <https://cloudinit.readthedocs.io/en/latest>`_ .

Using the live server image
===========================

Installing live server image
----------------------------

1. Extract the image (replacing 'ubuntu-22.04.5' with your release):

  .. code-block:: text

    gzip -d ubuntu-22.04.5-live-server-riscv64.img.gz

2. Create the disk image on which you will install Ubuntu. 16 GiB should be enough.

  .. code-block:: text

    dd if=/dev/zero bs=1M of=disk count=1 seek=16383

3. Start the installer with:

  .. code-block:: text

    /usr/bin/qemu-system-riscv64 -machine virt -m 4G -smp cpus=2 -nographic \
        -bios /usr/lib/riscv64-linux-gnu/opensbi/generic/fw_jump.bin \
        -kernel /usr/lib/u-boot/qemu-riscv64_smode/u-boot.bin \
        -netdev user,id=net0 \
        -device virtio-net-device,netdev=net0 \
        -drive file=ubuntu-22.04.5-live-server-riscv64.img,format=raw,if=virtio \
        -drive file=disk,format=raw,if=virtio \
        -device virtio-rng-pci

4. Follow the installation steps in `https://ubuntu.com/tutorials/install-ubuntu-server <https://ubuntu.com/tutorials/install-ubuntu-server>`_.

When rebooting we have to remove the installer image. Otherwise the installer will restart.

U-Boot gives you a 2 second time window to press the Enter key to reach the U-Boot console. In U-Boot’s console you can use the poweroff command to stop QEMU. Another option to exit QEMU is pressing keys CTRL-a followed by key x.

Running Ubuntu
--------------

To run your installed Ubuntu image use

.. code-block:: text

  /usr/bin/qemu-system-riscv64 -machine virt -m 4G -smp cpus=2 -nographic \
      -bios /usr/lib/riscv64-linux-gnu/opensbi/generic/fw_jump.bin \
      -kernel /usr/lib/u-boot/qemu-riscv64_smode/u-boot.bin \
      -netdev user,id=net0 \
      -device virtio-net-device,netdev=net0 \
      -drive file=disk,format=raw,if=virtio \
      -device virtio-rng-pci

Limitations
===========

* In Ubuntu 22.04 the number of virtual CPUs is limited to 8 in QEMU and in the Linux kernel.

