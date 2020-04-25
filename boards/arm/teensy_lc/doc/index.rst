.. _frdm_kl26z:

NXP FRDM-KL26Z
##############

Overview
********

The Freedom KL26Z is an ultra-low-cost development platform for
Kinetis® L series KL16 and KL26 MCUs (up to 128 KB Flash and up
to 64-pin packages) built on Arm® Cortex®-M0+ processor.

The FRDM-KL26Z features include easy access to MCU I/O, battery-ready,
low-power operation, a standard-based form factor with expansion board
options and a built-in debug interface for flash programming and run-control.

.. image:: ./frdm_kl26z.jpg
   :width: 272px
   :align: center
   :alt: FRDM-KL26Z

Hardware
********

- MKL26Z128VLH4 MCU @ 48 MHz, 128 KB flash, 16 KB SRAM, USB OTG (FS), 64LQFP
- On board capacitive touch "slider", FXOS8700CQ accelerometer, and tri-color LED
- OpenSDA debug interface

For more information about the KL26Z SoC and FRDM-KL26Z board:

- `KL26Z Website`_
- `KL26Z Datasheet`_
- `KL26Z Reference Manual`_
- `FRDM-KL26Z Website`_
- `FRDM-KL26Z User Guide`_
- `FRDM-KL26Z Schematics`_

Supported Features
==================

# TODO be sure

The frdm_kl26z board configuration supports the following hardware features:

+-----------+------------+-------------------------------------+
| Interface | Controller | Driver/Component                    |
+===========+============+=====================================+
| NVIC      | on-chip    | nested vector interrupt controller  |
+-----------+------------+-------------------------------------+
| SYSTICK   | on-chip    | systick                             |
+-----------+------------+-------------------------------------+
| PINMUX    | on-chip    | pinmux                              |
+-----------+------------+-------------------------------------+
| GPIO      | on-chip    | gpio                                |
+-----------+------------+-------------------------------------+
| UART      | on-chip    | serial port-polling;                |
|           |            | serial port-interrupt               |
+-----------+------------+-------------------------------------+
| I2C       | on-chip    | i2c                                 |
+-----------+------------+-------------------------------------+
| ADC       | on-chip    | adc                                 |
+-----------+------------+-------------------------------------+
| FLASH     | on-chip    | soc flash                           |
+-----------+------------+-------------------------------------+
| USB       | on-chip    | USB device                          |
+-----------+------------+-------------------------------------+

The default configuration can be found in the defconfig file:

	``boards/arm/frdm_kl26z/frdm_kl26z_defconfig``

Other hardware features are not currently supported by the port.

Connections and IOs
===================

# TODO be sure

The KL26Z SoC has five pairs of pinmux/gpio controllers, and all are currently enabled
(PORTA/GPIOA, PORTB/GPIOB, PORTC/GPIOC, PORTD/GPIOD, and PORTE/GPIOE) for the FRDM-KL26Z board.

+-------+-------------+---------------------------+
| Name  | Function    | Usage                     |
+=======+=============+===========================+
| PTB2  | ADC         | ADC0 channel 12           |
+-------+-------------+---------------------------+
| PTB18 | GPIO        | Red LED                   |
+-------+-------------+---------------------------+
| PTB19 | GPIO        | Green LED                 |
+-------+-------------+---------------------------+
| PTD1  | GPIO        | Blue LED                  |
+-------+-------------+---------------------------+
| PTA1  | UART0_RX    | UART Console              |
+-------+-------------+---------------------------+
| PTA2  | UART0_TX    | UART Console              |
+-------+-------------+---------------------------+
| PTE24 | I2C0_SCL    | I2C                       |
+-------+-------------+---------------------------+
| PTE25 | I2C0_SDA    | I2C                       |
+-------+-------------+---------------------------+


System Clock
============

# TODO be sure

The KL26Z SoC is configured to use the 8 MHz external oscillator on the board
with the on-chip FLL to generate a 48 MHz system clock.

Serial Port
===========

The KL26Z UART0 is used for the console.

USB
===

The KL26Z SoC has a USB OTG (USBOTG) controller that supports both
device and host functions through its mini USB connector (USB KL26Z).
Only USB device function is supported in Zephyr at the moment.

Programming and Debugging
*************************

Build and flash applications as usual (see :ref:`build_an_application` and
:ref:`application_run` for more details).

Configuring a Debug Probe
=========================

A debug probe is used for both flashing and debugging the board. This board is
configured by default to use the :ref:`opensda-daplink-onboard-debug-probe`.

Early versions of this board have an outdated version of the OpenSDA bootloader
and require an update. Please see the `DAPLink Bootloader Update`_ page for
instructions to update from the CMSIS-DAP bootloader to the DAPLink bootloader.

Option 1: :ref:`opensda-daplink-onboard-debug-probe` (Recommended)
------------------------------------------------------------------

Install the :ref:`pyocd-debug-host-tools` and make sure they are in your search
path.

Follow the instructions in :ref:`opensda-daplink-onboard-debug-probe` to program
the `OpenSDA DAPLink FRDM-KL26Z Firmware`_.

Option 2: :ref:`opensda-jlink-onboard-debug-probe`
--------------------------------------------------

Install the :ref:`jlink-debug-host-tools` and make sure they are in your search
path.

Follow the instructions in :ref:`opensda-jlink-onboard-debug-probe` to program
the `OpenSDA J-Link FRDM-KL26Z Firmware`_.

Add the argument ``-DOPENSDA_FW=jlink`` when you invoke ``west build`` to
override the default runner from pyOCD to J-Link:

.. zephyr-app-commands::
   :zephyr-app: samples/hello_world
   :board: frdm_kl26z
   :gen-args: -DOPENSDA_FW=jlink
   :goals: build

Configuring a Console
=====================

Regardless of your choice in debug probe, we will use the OpenSDA
microcontroller as a usb-to-serial adapter for the serial console.

Connect a USB cable from your PC to J7.

Use the following settings with your serial terminal of choice (minicom, putty,
etc.):

- Speed: 115200
- Data: 8 bits
- Parity: None
- Stop bits: 1

Flashing
========

Here is an example for the :ref:`hello_world` application.

.. zephyr-app-commands::
   :zephyr-app: samples/hello_world
   :board: frdm_kl26z
   :goals: flash

Open a serial terminal, reset the board (press the SW1 button), and you should
see the following message in the terminal:

.. code-block:: console

   ***** Booting Zephyr OS v1.14.0-rc1 *****
   Hello World! frdm_kl26z

Debugging
=========

Here is an example for the :ref:`hello_world` application.

.. zephyr-app-commands::
   :zephyr-app: samples/hello_world
   :board: frdm_kl26z
   :goals: debug

Open a serial terminal, step through the application in your debugger, and you
should see the following message in the terminal:

.. code-block:: console

   ***** Booting Zephyr OS v1.14.0-rc1 *****
   Hello World! frdm_kl26z

.. _FRDM-KL26Z Website:
   https://www.nxp.com/design/development-boards/freedom-development-boards/mcu-boards/freedom-development-platform-for-kinetis-kl16-and-kl26-mcus-up-to-128-kb-flash:FRDM-KL26Z

.. _FRDM-KL26Z User Guide:
   https://www.nxp.com/docs/en/user-guide/FRDMKL26ZUM.zip

.. _FRDM-KL26Z Schematics:
   https://www.nxp.com/downloads/en/schematics/FRDM-KL26Z_SCH_REV_B.pdf

.. _KL26Z Website:
   https://www.nxp.com/products/processors-and-microcontrollers/arm-based-processors-and-mcus/kinetis-cortex-m-mcus/l-seriesultra-low-powerm0-plus/kinetis-kl2x-72-96mhz-usb-ultra-low-power-microcontrollers-mcus-based-on-arm-cortex-m0-plus-core:KL2x?&l

.. _KL26Z Datasheet:
   https://www.nxp.com/docs/en/data-sheet/KL26P121M48SF4.pdf

.. _KL26Z Reference Manual:
   https://www.nxp.com/docs/en/reference-manual/KL26P121M48SF4RM.pdf

.. _DAPLink Bootloader Update:
   https://os.mbed.com/blog/entry/DAPLink-bootloader-update/

.. _OpenSDA J-Link FRDM-KL26Z Firmware:
   https://www.segger.com/downloads/jlink/OpenSDA_FRDM-KL26Z
