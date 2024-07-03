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

    TTL
        `Transistor-transistor logic
        <https://en.wikipedia.org/wiki/Transistor%E2%80%93transistor_logic>`_;
        a family of logic circuits. In the context of serial adapters and
        :term:`UART` consoles, however, this usually refers to the voltage
        level expected.

        The `RS-232 <https://en.wikipedia.org/wiki/RS-232>`_ standard (commonly
        used in serial interfaces) permits a wide range of voltages from 3V up
        to 15V. However, the UARTs on most :term:`SBCs <SBC>` expect no more
        than the 3.3V typically used on their :term:`GPIO` headers. Thus, "TTL
        UART" or "serial TTL" implies a UART interface (or adapter) that
        requires (or accepts) no more than 3.3V through its pins.

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
