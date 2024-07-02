===========
Glossary
===========


.. glossary::

    API
        An `Application Programming Interface
        <https://en.wikipedia.org/wiki/API>`_

    EBBR
        The `Embedded Base Boot Requirements
        <https://arm-software.github.io/ebbr/index.html>`_ specification.

    eMMC
        `Embedded MultiMediaCard
        <https://en.wikipedia.org/wiki/MultiMediaCard#eMMC>`_, a storage
        standard that puts the components of a MultiMediaCard in an integrated
        circuit for use in circuit boards as non-volatile storage.

    GPIO
        General Purpose Input/Output; in the context of :term:`SBCs <SBC>`,
        this typically refers to the prominent pin-header (often containing
        2×20 pins) along one edge of the board. Some (but not all) of these
        pins may be controlled by software, operating as inputs or outputs.

    GRUB
        The GNU `GRand Unified Bootloader
        <https://www.gnu.org/software/grub/>`_, commonly used as the Linux
        bootloader on PC platforms, and a few :term:`SBCs <SBC>` too.

    QEMU
        The `Quick Emulator <https://www.qemu.org/>`_ project, a popular
        virtual machine implementation supporting multiple architectures and
        cross-architectural emulation under Linux/BSD.

    SBC
        A Single Board Computer.

    UART
        `Universal Asynchronous receiver-transmitter
        <https://en.wikipedia.org/wiki/Universal_asynchronous_receiver-transmitter>`_;
        the acronym most commonly used to reference a device's `serial port
        <https://en.wikipedia.org/wiki/Serial_port>`_ over which a Linux
        console may be accessed.

        Frequently used for debugging early boot problems, as the serial port
        is a much simpler device than video output, and thus less likely to
        fail in the event of kernel bugs.

    UEFI
        `Unified Extensible Firmware Interface
        <https://en.wikipedia.org/wiki/UEFI>`_ is a specification for the
        booting of computer hardware. Whilst nearly ubiquitous on modern PCs,
        support for it is considerably more sparse on :term:`SBCs <SBC>`.
