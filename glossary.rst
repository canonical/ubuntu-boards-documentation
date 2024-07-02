.. glossary::

    EBBR
        The `Embedded Base Boot Requirements
        <https://arm-software.github.io/ebbr/index.html>`_ specification.

    GPIO
        General Purpose Input/Output; in the context of :term:`SBCs <SBC>`,
        this typically refers to the prominent pin-header (often containing
        2×20 pins) along one edge of the board. Some (but not all) of these
        pins may be controlled by software, operating as inputs or outputs.

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
