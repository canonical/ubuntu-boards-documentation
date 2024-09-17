=========================
Connect to a UART console
=========================

Serial UART consoles permit communication between your computer and your board. You can use the package picocom to access the serial console. Here is an example for a baudrate of 115200 and the UART device connected at /dev/ttyUSB:

.. code-block:: text

    sudo apt-get install picocom
    sudo picocom -b 115200 /dev/ttyUSB

In general you should replace the baud rate ``-b 115200`` to the one specified by your board, and ``/dev/ttyUSB`` by your device.  