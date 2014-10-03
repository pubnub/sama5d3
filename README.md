sama5d3
=======

PubNub for Atmel's SAMA5D3 Usage Notes and Client SDK Examples

The SAMA5D3 ARM processor is capable of running Linux and transitively
many of our SDKs.  In our instructions, we will focus primarily on
setting up the C SDK.

We document setup of two Linux platforms on the "SAMA5D3 Xplained"
development board - this should also serve as a good base for any
other SAMA5D3-based Linux platforms:

  * [Debian](HOWTO-Debian.md) is a versatile, extremely popular Linux
    distribution that offers package management and comfortable
    development environment on all platforms from big iron to embedded.
    Debian boots from an SD card.

  * [Poky](HOWTO-Poky.md) is an industry standard for cross-compiling
    the whole Linux system, from source and custom tailored to your
    hardware and specific needs.  Development is more involved but
    there is minimum overhead in the final product.  Poky runs from
    the internal NAND flash.
