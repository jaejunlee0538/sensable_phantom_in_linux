**Date**

* 2015.6.7

**OS versions**

* Ubuntu 14.04 LTS 32bit 3.4.6-xenomai-2.6.2.1

> NVDIA GTX570 was not compatable with `3.4.6-xenomai-2.6.2.1`.
It was booted in **Low-Graphic Mode**.

**Hardware Specification**

|           |Specification                              |
|-------    |-----------------------------------------  |
|CPU        |Intel i5-4690K CPU 3.50GHz x 4             |
|Memory     |16 GB                                      |
|GPU        |Gallium 0.4 on NV92                        |
|MainBoard  |GIGABYTE GA-P85-D3                         |

**Communication Devices Compatibility**

>|                           |Omni(1394)|Premium 1.5A(Parport)    | Premium 3.0 6DOF(Parport)|
|---------------------------|:-------:  |:--------------------:  |:------------------------:|
|NEXT-1394NEC PCI           | **Good**  |  N                     | N                         |
|Built-in parport(GA-P85-D3)| N         | **Good**               | N                         |

**Note**

* **Xenomai** : Sometime, Xenomai cannot be booted well.
* **Error** : This error has occured one time. Now everything is working.

![xenomai_phantom_error1]

![xenomai_phantom_error2]

* **PHANToMTest** : Sometimes graphic rendering at Calibrate and Read Encoder steps became quite slow. But Box-Test and Test-Force step had no problem.


* **Example Code with Omni** : Couldn't start example code with Phantom Omni.

```shell
$ ./CoulombField 
Starting application
****************************************
This is the ACADEMIC EDITION of OPENHAPTICS, commercial distribution is prohibited
Please contact SensAble Technologies to obtain a commercial license
****************************************
HD Error: HD_INVALID_VALUE
Invalid value specified for a function that is attempting to set a value.
HHD: FFFFFFFF
Error Code: 101
Internal Error Code: -5
Message: Failed to initialize haptic device

Press any key to quit.
```

* **Example Code with Premium** : It worked well, but sometimes graphic rendering paused shortly and started again.

* **IRQ** : firewire_ohci and nouveau share same interrupt. Will it be a problem?

```shell
$ cat /proc/interrupts 
           CPU0       CPU1       CPU2       CPU3       
  0:         42          0          0          0   IO-APIC-edge      timer
  1:       8266          0          0          0   IO-APIC-edge      i8042
  5:     500734          0          0          0   IO-APIC-edge      parport0
  8:          1          0          0          0   IO-APIC-edge      rtc0
  9:          0          0          0          0   IO-APIC-fasteoi   acpi
 16:     505241          0          0          0   IO-APIC-fasteoi   ehci_hcd:usb1, firewire_ohci, nouveau, snd_hda_intel
 19:         28          0          0          0   IO-APIC-fasteoi 
 23:         92          0          0      40766   IO-APIC-fasteoi   ehci_hcd:usb2
 41:         82          0          0      20878   PCI-MSI-edge      eth0
 42:      18322       4488          0       8827   PCI-MSI-edge      ahci
 43:        727          0          0          0   PCI-MSI-edge      snd_hda_intel
NMI:        193        117        117        120   Non-maskable interrupts
LOC:     312497     380739     437997     458985   Local timer interrupts
SPU:          0          0          0          0   Spurious interrupts
PMI:        193        117        117        120   Performance monitoring interrupts
IWI:          0          0          0          0   IRQ work interrupts
RTR:          3          0          0          0   APIC ICR read retries
RES:      34517       5349       4535       2189   Rescheduling interrupts
CAL:        258        386        418        476   Function call interrupts
TLB:       9047      11123       9536      10760   TLB shootdowns
TRM:          0          0          0          0   Thermal event interrupts
THR:          0          0          0          0   Threshold APIC interrupts
MCE:          0          0          0          0   Machine check exceptions
MCP:         12         12         12         12   Machine check polls
ERR:          0
MIS:          0
```

```shell
$  lspci -v
00:00.0 Host bridge: Intel Corporation 4th Gen Core Processor DRAM Controller (rev 06)
	Subsystem: Gigabyte Technology Co., Ltd Device 5000
	Flags: bus master, fast devsel, latency 0
	Capabilities: <access denied>

00:01.0 PCI bridge: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor PCI Express x16 Controller (rev 06) (prog-if 00 [Normal decode])
	Flags: bus master, fast devsel, latency 0
	Bus: primary=00, secondary=01, subordinate=01, sec-latency=0
	I/O behind bridge: 0000e000-0000efff
	Memory behind bridge: f4000000-f70fffff
	Prefetchable memory behind bridge: 00000000e0000000-00000000efffffff
	Capabilities: <access denied>
	Kernel driver in use: pcieport

00:02.0 VGA compatible controller: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor Integrated Graphics Controller (rev 06) (prog-if 00 [VGA controller])
	Subsystem: Gigabyte Technology Co., Ltd Device d000
	Flags: bus master, fast devsel, latency 0, IRQ 11
	Memory at f7400000 (64-bit, non-prefetchable) [disabled] [size=4M]
	Memory at d0000000 (64-bit, prefetchable) [disabled] [size=256M]
	I/O ports at f000 [disabled] [size=64]
	Expansion ROM at <unassigned> [disabled]
	Capabilities: <access denied>

00:03.0 Audio device: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor HD Audio Controller (rev 06)
	Subsystem: Intel Corporation Device 2010
	Flags: bus master, fast devsel, latency 0, IRQ 16
	Memory at f7a14000 (64-bit, non-prefetchable) [size=16K]
	Capabilities: <access denied>
	Kernel driver in use: snd_hda_intel

00:14.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB xHCI (rev 05) (prog-if 30 [XHCI])
	Subsystem: Gigabyte Technology Co., Ltd Device 5007
	Flags: medium devsel, IRQ 16
	Memory at f7a00000 (64-bit, non-prefetchable) [size=64K]
	Capabilities: <access denied>

00:16.0 Communication controller: Intel Corporation 8 Series/C220 Series Chipset Family MEI Controller #1 (rev 04)
	Subsystem: Gigabyte Technology Co., Ltd Device 1c3a
	Flags: bus master, fast devsel, latency 0, IRQ 11
	Memory at f7a1f000 (64-bit, non-prefetchable) [size=16]
	Capabilities: <access denied>

00:16.3 Serial controller: Intel Corporation 8 Series/C220 Series Chipset Family KT Controller (rev 04) (prog-if 02 [16550])
	Subsystem: Gigabyte Technology Co., Ltd Device 1c3a
	Flags: bus master, 66MHz, fast devsel, latency 0, IRQ 19
	I/O ports at f0c0 [size=8]
	Memory at f7a1d000 (32-bit, non-prefetchable) [size=4K]
	Capabilities: <access denied>
	Kernel driver in use: serial

00:1a.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB EHCI #2 (rev 05) (prog-if 20 [EHCI])
	Subsystem: Gigabyte Technology Co., Ltd Device 5006
	Flags: bus master, medium devsel, latency 0, IRQ 16
	Memory at f7a1c000 (32-bit, non-prefetchable) [size=1K]
	Capabilities: <access denied>
	Kernel driver in use: ehci_hcd

00:1b.0 Audio device: Intel Corporation 8 Series/C220 Series Chipset High Definition Audio Controller (rev 05)
	Subsystem: Gigabyte Technology Co., Ltd Device a002
	Flags: bus master, fast devsel, latency 0, IRQ 43
	Memory at f7a10000 (64-bit, non-prefetchable) [size=16K]
	Capabilities: <access denied>
	Kernel driver in use: snd_hda_intel

00:1c.0 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #1 (rev d5) (prog-if 00 [Normal decode])
	Flags: bus master, fast devsel, latency 0
	Bus: primary=00, secondary=02, subordinate=02, sec-latency=0
	Capabilities: <access denied>
	Kernel driver in use: pcieport

00:1c.2 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #3 (rev d5) (prog-if 00 [Normal decode])
	Flags: bus master, fast devsel, latency 0
	Bus: primary=00, secondary=03, subordinate=03, sec-latency=0
	I/O behind bridge: 0000d000-0000dfff
	Memory behind bridge: f7900000-f79fffff
	Prefetchable memory behind bridge: 00000000f0000000-00000000f00fffff
	Capabilities: <access denied>
	Kernel driver in use: pcieport

00:1c.3 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #4 (rev d5) (prog-if 00 [Normal decode])
	Flags: bus master, fast devsel, latency 0
	Bus: primary=00, secondary=04, subordinate=05, sec-latency=0
	Memory behind bridge: f7800000-f78fffff
	Capabilities: <access denied>
	Kernel driver in use: pcieport

00:1d.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB EHCI #1 (rev 05) (prog-if 20 [EHCI])
	Subsystem: Gigabyte Technology Co., Ltd Device 5006
	Flags: bus master, medium devsel, latency 0, IRQ 23
	Memory at f7a1b000 (32-bit, non-prefetchable) [size=1K]
	Capabilities: <access denied>
	Kernel driver in use: ehci_hcd

00:1f.0 ISA bridge: Intel Corporation B85 Express LPC Controller (rev 05)
	Subsystem: Gigabyte Technology Co., Ltd Device 5001
	Flags: bus master, medium devsel, latency 0
	Capabilities: <access denied>

00:1f.2 SATA controller: Intel Corporation 8 Series/C220 Series Chipset Family 6-port SATA Controller 1 [AHCI mode] (rev 05) (prog-if 01 [AHCI 1.0])
	Subsystem: Gigabyte Technology Co., Ltd Device b005
	Flags: bus master, 66MHz, medium devsel, latency 0, IRQ 42
	I/O ports at f0b0 [size=8]
	I/O ports at f0a0 [size=4]
	I/O ports at f090 [size=8]
	I/O ports at f080 [size=4]
	I/O ports at f060 [size=32]
	Memory at f7a1a000 (32-bit, non-prefetchable) [size=2K]
	Capabilities: <access denied>
	Kernel driver in use: ahci

00:1f.3 SMBus: Intel Corporation 8 Series/C220 Series Chipset Family SMBus Controller (rev 05)
	Subsystem: Gigabyte Technology Co., Ltd Device 5001
	Flags: medium devsel, IRQ 10
	Memory at f7a19000 (64-bit, non-prefetchable) [disabled] [size=256]
	I/O ports at f040 [size=32]

01:00.0 VGA compatible controller: NVIDIA Corporation G92 [GeForce 9800 GT] (rev a2) (prog-if 00 [VGA controller])
	Flags: bus master, fast devsel, latency 0, IRQ 16
	Memory at f6000000 (32-bit, non-prefetchable) [size=16M]
	Memory at e0000000 (64-bit, prefetchable) [size=256M]
	Memory at f4000000 (64-bit, non-prefetchable) [size=32M]
	I/O ports at e000 [size=128]
	Expansion ROM at f7000000 [disabled] [size=128K]
	Capabilities: <access denied>
	Kernel driver in use: nouveau

03:00.0 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller (rev 06)
	Subsystem: Gigabyte Technology Co., Ltd Motherboard
	Flags: bus master, fast devsel, latency 0, IRQ 41
	I/O ports at d000 [size=256]
	Memory at f7900000 (64-bit, non-prefetchable) [size=4K]
	Memory at f0000000 (64-bit, prefetchable) [size=16K]
	Capabilities: <access denied>
	Kernel driver in use: r8169

04:00.0 PCI bridge: Intel Corporation 82801 PCI Bridge (rev 41) (prog-if 01 [Subtractive decode])
	Flags: bus master, fast devsel, latency 0
	Bus: primary=04, secondary=05, subordinate=05, sec-latency=32
	Memory behind bridge: f7800000-f78fffff
	Capabilities: <access denied>

05:01.0 FireWire (IEEE 1394): Texas Instruments TSB43AB23 IEEE-1394a-2000 Controller (PHY/Link) (prog-if 10 [OHCI])
	Subsystem: Texas Instruments TSB43AB23 IEEE-1394a-2000 Controller (PHY/Link)
	Flags: bus master, medium devsel, latency 32, IRQ 16
	Memory at f7826000 (32-bit, non-prefetchable) [size=2K]
	Memory at f7820000 (32-bit, non-prefetchable) [size=16K]
	Capabilities: <access denied>
	Kernel driver in use: firewire_ohci

05:02.0 Network controller: PEAK-System Technik GmbH PCAN-PCI CAN-Bus controller (rev 02)
	Subsystem: PEAK-System Technik GmbH 2 Channel CAN Bus SJC1000 (Optically Isolated)
	Flags: medium devsel, IRQ 17
	Memory at f7810000 (32-bit, non-prefetchable) [size=64K]
	Memory at f7800000 (32-bit, non-prefetchable) [size=64K]
	Kernel driver in use: peak_pci

05:03.0 Unassigned class [ff00]: National Instruments PCI-6220
	Flags: bus master, medium devsel, latency 32, IRQ 10
	Memory at f7825000 (32-bit, non-prefetchable) [size=4K]
	Memory at f7824000 (32-bit, non-prefetchable) [size=4K]
```




[xenomai_phantom_error1]:https://raw.github.com/jaejunlee0538/sensable_phantom_in_linux/master/resources/xenomai_phantom_error1.png
[xenomai_phantom_error2]:https://raw.github.com/jaejunlee0538/sensable_phantom_in_linux/master/resources/xenomai_phantom_error2.png
