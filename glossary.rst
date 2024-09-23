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
        `General Purpose Input/Output
        <https://en.wikipedia.org/wiki/General-purpose_input/output>`_; in the
        context of :term:`SBCs <SBC>`, this typically refers to the prominent
        pin-header (often containing 2×20 pins) along one edge of the board.
        Some (but not all) of these pins may be controlled by software,
        operating as inputs or outputs.

    GRUB
        The GNU `GRand Unified Bootloader
        <https://www.gnu.org/software/grub/>`_, commonly used as the Linux
        bootloader on PC platforms, and a few :term:`SBCs <SBC>` too.

    HAT
        Hardware Attached on Top; the Raspberry Pi `specification
        <https://github.com/raspberrypi/hats>`_ guiding the design of boards
        intended to connect to the 40-pin :term:`GPIO` header, and sit "on top"
        of the Pi. With the advent of the Pi 5, the specification is now
        superseded by the `HAT+ specification
        <https://datasheets.raspberrypi.com/hat/hat-plus-specification.pdf>`_.

    JTAG
        An industry standard developed by the Joint Test Action Group (JTAG)
        for verification and testing of circuit boards. In the context of
        :term:`SBCs <SBC>`, typically refers to the physical `debug interface
        <https://en.wikipedia.org/wiki/JTAG#Electrical_characteristics>`_
        found on many boards.

    Netplan
        `Netplan <https://netplan.io/>`_ is a utility for easily configuring networking on a linux system. 
        You simply create a YAML description of the required network interfaces 
        and what each should be configured to do. 

    NVMe
        `Non-Volatile Memory express
        <https://en.wikipedia.org/wiki/NVM_Express>`_; the interface used by
        modern flash storage devices, developed to more closely match the
        capabilities and behaviour of flash storage (in contrast to interfaces
        like :term:`SATA` which were developed for use with spinning media).

    QEMU
        The `Quick Emulator <https://www.qemu.org/>`_ project, a popular
        virtual machine implementation supporting multiple architectures and
        cross-architectural emulation under Linux/BSD.

    SATA
        `Serial AT Attachment <https://en.wikipedia.org/wiki/SATA>`_; an
        interface used by hard disk and flash storage drives, though now
        largely superseded by :term:`NVMe` in the case of flash storage.

    SBC
        A Single Board Computer.

    SSD
        `Solid State Drive <https://en.wikipedia.org/wiki/Solid-state_drive>`_;
        a type of persistent storage drive that has no moving parts, relying
        instead on flash memory.

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
