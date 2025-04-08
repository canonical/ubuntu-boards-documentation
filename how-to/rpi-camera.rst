================================
Using the camera on Raspberry Pi
================================

Since Ubuntu 25.04, all `Raspberry Pi`_ models with a CSI port can use the camera stack.

Supported Camera modules
========================

We support all the official Raspberry Pi camera modules listed on the `Raspberry Pi camera hardware spec`_.

Also, the current ``libcamera`` in Ubuntu supports the following camera sensors:

- IMX219
- IMX283
- IMX290
- IMX296
- IMX327
- IMX378
- IMX415
- IMX462
- IMX477
- IMX500
- IMX519
- IMX708
- OV5647
- OV64A40
- OV7251
- OV9281
- SE327M12

Connecting a camera module
==========================

You can follow the Raspberry Pi's official documentation to `connect a supported camera module`_ to your Raspberry Pi.

.. caution::
    We do not recommend connecting a camera module while the Raspberry Pi is running,
    and you may experience erratic behaviour or unintended reboots if you try it.
    Please plug the camera module while the Raspberry Pi is powered off.

libcamera-apps
==============

Right after installing Ubuntu on your Raspberry Pi and connecting a camera module to it,
you can use the various applications provided by the `libcamera-apps` package to test the camera. We will show the usage of
2 of the applications here: ``cam`` and ``qcam``.

`cam`_
------

.. tip::
    We recommend setting the ``LIBCAMERA_LOG_LEVELS`` to 4 using
    ``export LIBCAMERA_LOG_LEVELS=*:4`` to avoid cluttering the terminal with logs.

The ``cam`` utility can be used for basic testing.

You can list the connected cameras using ``cam -l``.

.. terminal::
    :input: cam -l
    :user: r41k0u
    :host: ubuntu-pi5

    Available cameras:
    1: 'imx500' (/base/axi/pcie@120000/rp1/i2c@80000/imx500@1a)

You can also capture frames using ``cam`` in the PPM format.

.. terminal::
    :input: cam -c 1 --capture --file=frame.ppm
    :user: r41k0u
    :host: ubuntu-pi5

    Using camera /base/axi/pcie@120000/rp1/i2c@80000/imx500@1a as cam0
    cam0: Capture 1 frames
    1237.987399 (0.00 fps) cam0-stream0 seq: 000007 bytesused: 1920000


qcam
----

qcam is a GUI application provided by libcamera that can be used to test the camera. You can use
it by just typing ``qcam`` in the terminal.

.. terminal::
    :input: qcam
    :user: r41k0u
    :host: ubuntu-pi5

    Zero-copy enabled

.. figure:: /images/qcam-demo.png
    :alt: qcam viewfinder window
    
    Testing your camera using qcam.

- The red cross at the top left exits ``qcam``.
- For Raspberry Pi models supporting more than one camera module, you can select another camera by clicking
  on the textbox that shows the camera name.

.. figure:: /images/qcam-dropdown-demo.png
    :alt: qcam sensor selection
    
    Select your camera using the dropdown menu.

- You can use the ``Stop Capture`` button to turn off the camera viewfinder, and toggle it again to start it.
- The blue arrow can be used to take a snapshot of the current viewfinder image as a PNG.
- The ``Save Raw`` button can be used to capture a RAW image.

rpicam-apps
===========

.. important::
    While the AI Camera module is supported in Ubuntu, the post-processing stages require the imx500-firmware
    package, which can't be included in Ubuntu due to licensing issues. So anything in the `AI camera documentation`_ might
    not work out of the box. We are working on fixing the licensing issues with the involved parties.

You can install rpicam-apps on your Raspberry Pi with the following command:

.. terminal::
    :input: sudo apt install rpicam-apps
    :user: r41k0u
    :host: ubuntu-pi5


After this, you can use rpicam-apps on Ubuntu the exact same way as you would on Raspberry Pi OS.
To do that, you can follow the `rpicam-apps documentation`_. 

.. LINKS
.. _Raspberry Pi: https://www.raspberrypi.com/
.. _Raspberry Pi camera hardware spec: https://www.raspberrypi.com/documentation/accessories/camera.html#hardware-specification
.. _connect a supported camera module: https://www.raspberrypi.com/documentation/accessories/camera.html#connect-the-camera
.. _cam: https://libcamera.org/getting-started.html#basic-testing-with-cam-utility
.. _AI camera documentation: https://www.raspberrypi.com/documentation/accessories/ai-camera.html
.. _rpicam-apps documentation: https://www.raspberrypi.com/documentation/computers/camera_software.html#rpicam-apps