==============================
Flash images to a microSD card
==============================

Images are usually downloaded in a compressed form of the type ``<image-name>.img.gz``. An example would be ``ubuntu-22.04.2-live-server-riscv64.img.gz``. Such image can be decompressed and installed in a microSD card with the command:

.. code-block:: text

    zcat ubuntu-22.04.2-live-server-riscv64.img.gz | sudo dd of=/dev/sdX bs=1M conv=fsync

Where you will have to replace /dev/sdX by the actual device name of your SD card. Please, be especially cautious not to overwrite the wrong drive as this cannot be undone.