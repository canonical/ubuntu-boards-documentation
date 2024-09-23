=========================
Connect to a UART console
=========================

Serial UART consoles permit communication between your computer and your board. You can use the package picocom to access the serial console. Here is an example for a baudrate of 115200 and the UART device connected at /dev/ttyUSB:

.. code-block:: text

    sudo apt-get install picocom
    sudo picocom -b 115200 /dev/ttyUSB0

In general you should replace the baud rate ``-b 115200`` to the one specified by your board, and ``/dev/ttyUSB0`` by your device. Note that serial interfaces are accessible by the root user and by users in the ``dialout`` group. The following adds an user to the dialout group:

.. code-block:: text

    sudo usermod -aG dialout <username>
