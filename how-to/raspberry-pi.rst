==================================
Install Ubuntu on the Raspberry Pi
==================================

The `Raspberry Pi`_ is an ARM based :term:`SBC` made in the UK.


Supported models
================

The following matrix maps all models of the Raspberry Pi against the releases
of Ubuntu that support them.

+--------------+-----------+-------------+--------------+-------------+
| Raspberry Pi | 20.04 LTS | 22.04 LTS   | 24.04 LTS    | 24.10       |
+==============+===========+=============+==============+=============+
| `B+`_        |           |             |              |             |
+--------------+-----------+-------------+--------------+-------------+
| `A+`_        |           |             |              |             |
+--------------+-----------+-------------+--------------+-------------+
| `2B`_        | S         | S           | [#2B64]_     | [#2B64]_    |
+--------------+-----------+-------------+--------------+-------------+
| `3B`_        | S         | S           | S            | S           |
+--------------+-----------+-------------+--------------+-------------+
| `3B+`_       | S         | S           | S            | S           |
+--------------+-----------+-------------+--------------+-------------+
| `3A+`_       | S         | S           | S            | S           |
+--------------+-----------+-------------+--------------+-------------+
| `4B`_        | S         | SD [#RAM2]_ | SDC [#RAM4]_ | SD [#RAM4]_ |
+--------------+-----------+-------------+--------------+-------------+
| `400`_       |           | SD          | SD           | SD          |
+--------------+-----------+-------------+--------------+-------------+
| `5­`_        |           |             | SDC [#RAM4]_ | SD [#RAM4]_ |
+--------------+-----------+-------------+--------------+-------------+
| `Zero`_      |           |             |              |             |
+--------------+-----------+-------------+--------------+-------------+
| `Zero W`_    |           |             |              |             |
+--------------+-----------+-------------+--------------+-------------+
| `Zero 2W`_   | S         | S           | S            | S           |
+--------------+-----------+-------------+--------------+-------------+
| `CM1`_       |           |             |              |             |
+--------------+-----------+-------------+--------------+-------------+
| `CM3`_       | S         | [#CM3MMC]_  |              |             |
+--------------+-----------+-------------+--------------+-------------+
| `CM3+`_      | S         | S           | S            | S           |
+--------------+-----------+-------------+--------------+-------------+
| `CM4`_       |           | SD [#RAM2]_ | SDC [#RAM4]_ | SD [#RAM4]_ |
+--------------+-----------+-------------+--------------+-------------+
| `CM5`_       |           |             | [#Noble.2]_  | SD [#RAM4]_ |
+--------------+-----------+-------------+--------------+-------------+

Key:

S
    Server supported
D
    Desktop supported
C
    `Certified`_ (LTS releases only)

.. _A+: https://www.raspberrypi.com/products/raspberry-pi-1-model-a-plus/
.. _B+: https://www.raspberrypi.com/products/raspberry-pi-1-model-b-plus/
.. _2B: https://www.raspberrypi.com/products/raspberry-pi-2-model-b/
.. _3B: https://www.raspberrypi.com/products/raspberry-pi-3-model-b/
.. _3B+: https://www.raspberrypi.com/products/raspberry-pi-3-model-b-plus/
.. _3A+: https://www.raspberrypi.com/products/raspberry-pi-3-model-a-plus/
.. _4B: https://www.raspberrypi.com/products/raspberry-pi-4-model-b/
.. _400: https://www.raspberrypi.com/products/raspberry-pi-400-unit/
.. _5­: https://www.raspberrypi.com/products/raspberry-pi-5/
.. _Zero: https://www.raspberrypi.com/products/raspberry-pi-zero/
.. _Zero W: https://www.raspberrypi.com/products/raspberry-pi-zero-w/
.. _Zero 2W: https://www.raspberrypi.com/products/raspberry-pi-zero-2-w/
.. _CM1: https://www.raspberrypi.com/products/compute-module-1/
.. _CM3: https://www.raspberrypi.com/products/compute-module-3/
.. _CM3+: https://www.raspberrypi.com/products/compute-module-3-plus/
.. _CM4: https://www.raspberrypi.com/products/compute-module-4/
.. _CM5: https://www.raspberrypi.com/products/compute-module-5/

.. [#2B64] The original 2B does not support the arm64 architecture, which is the
   only architecture supported on 24.04; the 1.2 board revision of the 2B
   *does* support arm64 and has been tested to work, but as this does not cover
   the majority of 2B models in the field, this is not a supported board as of
   24.04

.. [#CM3MMC] The Compute Module 3 is only provided with 4GB of :term:`eMMC` storage;
   as of 22.04 the base Ubuntu server image became too large to support on the
   CM3. Please use the CM3+ (which ships with 8GB of eMMC) as the replacement

.. [#RAM2] Desktop requires 2GB of RAM or more

.. [#RAM4] Desktop requires 4GB of RAM or more

.. [#Noble.2] CM5 support is planned for the 24.04.2 point release


Using the pre-installed server image
====================================

#. Download one of the supported images:

   .. ubuntu-images::
       :releases: focal-
       :suffix: +raspi
       :image-types: preinstalled-server

#. Flash the pre-installed image to your selection of boot media (microSD card,
   USB mass-storage device or, on the CM4 or Pi 5, :term:`NVMe` storage)

#. If desired, customize the `cloud-init`_ configuration on the boot partition,
   found in the :file:`user-data` and :file:`network-config` files

#. Attach the boot media to the board (for USB or NVMe this may require an
   adapter or :term:`HAT`)

#. Attach a screen and keyboard

#. Power on the board

#. Wait for an output line confirming that `cloud-init`_ has finished running;
   this service is responsible for generating SSH keys, and creating the
   default user:

   .. code-block:: text

       [   35.682018] cloud-init[909]: Cloud-init v. 24.1.3-0ubuntu3 finished at Tue, 23 Apr 2024 07:44:59 +0000. Datasource DataSourceNoCloud [seed=/var/lib/cloud/seed/nocloud-net][dsmode=net].  Up 35.65 seconds

#. Login with the user *ubuntu* and the default password *ubuntu* (if you left
   the cloud-init configuration with its defaults); you will be asked to choose
   a new password


Using the pre-installed desktop image
=====================================

#. Download one of the supported images:

   .. ubuntu-images::
       :releases: focal-
       :suffix: +raspi
       :image-types: preinstalled-desktop

#. Flash the pre-installed image to your selection of boot media (microSD card,
   USB mass-storage device or, on the CM4 or Pi 5, :term:`NVMe` storage)

#. Attach the boot media to the board (for USB or NVMe this may require an
   adapter or :term:`HAT`)

#. Attach a screen, keyboard, and mouse

#. Power on the board

#. Wait for the first-time setup wizard to start and follow the instructions


Cloud-init seed
===============

The cloud-init seed for the pre-installed Raspberry Pi images is always located
on the boot partition (the first partition in the image, formatted as FAT).
Please note that, prior to the 24.04 LTS (noble) release, only the
pre-installed server images provided cloud-init.

See :doc:`/how-to/headless-usage` for more information.


UART console
============

The :term:`UART` console is accessible via the :term:`GPIO` header on all
models *except* the Pi 5. Assuming the typical coloring of USB to :term:`TTL`
serial adapters the following connections have to be made:

=========== ==========
Board       Adapter
=========== ==========
GND, pin  6 GND, black
TX,  pin  8 RX,  white
RX,  pin 10 TX,  green
=========== ==========

Do not connect the red 3.3 V wire.

.. image:: /images/rpi-gpio-uart-traditional.jpg
    :alt: A close-up of a traditional USB UART adapter connected to the UART
          on the GPIO header of a Raspberry Pi 4B

If you are using a Raspberry Pi `debug probe`_, the coloring of wires is
slightly different, and you must ensure that the lead is connected to the "U"
port of the debug probe, but the connections on the GPIO header are similar:

=========== ===========
Board       Debug Probe
=========== ===========
GND, pin  6 GND, black
TX,  pin  8 RX,  yellow
RX,  pin 10 TX,  orange
=========== ===========

.. image:: /images/rpi-gpio-uart-debug-probe.jpg
    :alt: A close-up of the Raspberry Pi debug probe connected to the UART
          on the GPIO header of a Raspberry Pi 4B

On the Pi 5, the serial console is accessed via the 3-pin UART header between
the two micro-HDMI ports. The simplest means of using this UART is to use the
Raspberry Pi `debug probe`_ connected as shown:

.. image:: /images/rpi-debug-uart.jpg
    :alt: A close-up of the Raspberry Pi debug probe connected to the UART
          port located between the micro-HDMI ports of a Raspberry Pi 5.

If you wish to access the serial console via the GPIO header as on previous
models, append the following line to :file:`config.txt` on the boot partition:

.. code-block:: text

    dtparam=uart0_console

Connect with the following settings (see :doc:`/how-to/uart-console`):

* 115200 baud
* 8 data bits
* no parity
* 1 stop bit
* no flow control


Limitations
===========

* The libcamera stack is not currently operational on 24.04 (:lp-bug:`2038669`)


.. _Raspberry Pi: https://www.raspberrypi.com/
.. _Certified: https://certification.canonical.com/
.. _debug probe: https://www.raspberrypi.com/products/debug-probe/
