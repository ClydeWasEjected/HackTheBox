## Enumeration Principles
Enumeration is a term in cybersecurity that stands for information gathering using active and passive methods. OSINT is an independent procedure and it should be performed separately from enumeration because "OSINT is based exclusively on passive information gathering"

**Our goal is not to get at the systems but to find all the ways to get there.**

- What can we see?
- What reasons can we have for seeing it?
- What image does what we see create for us?
- What do we gain from it?
- How can we use it?
- What can we not see?
- What reasons can there be that we do not see?
- What image results for us from what we do not see?

| **`No.`** | **`Principle`**                                                        |
| --------- | ---------------------------------------------------------------------- |
| 1.        | There is more than meets the eye. Consider all points of view.         |
| 2.        | Distinguish between what we see and what we do not see.                |
| 3.        | There are always ways to gain more information. Understand the target. |

## Enumeration Methology

## IPMI
**Intelligent Platform Management Interface (IPMI)** is a system used for **remote management and monitoring** of servers. It works **independently from the main operating system**, meaning it still functions even if the server is powered off, frozen, or not responding.
### What IPMI allows admins to do

Admins can manage a system even when:

- The OS hasn’t booted yet (to change BIOS settings)
- The server is completely powered off
- The system has crashed or failed
IPMI works through a **direct network connection to the hardware**, not the operating system.

### What IPMI can monitor

When not being used for recovery tasks, it can monitor:
- Temperature
- Voltage
- Fan status
- Power supply health
It can also:
- Query hardware inventory
- Read hardware logs
- Send alerts through SNMP

Note: The server itself can be off, but the IPMI module still needs **power** and a **LAN connection**.


### Components required for IPMI

1. **BMC (Baseboard Management Controller)**  
    The main micro-controller that runs IPMI.
2. **ICMB (Intelligent Chassis Management Bus)**  
    Lets different chassis communicate with each other.
3. **IPMB (Intelligent Platform Management Bus)**  
    Extends the BMC to other components inside the system.
4. **IPMI Memory**  
    Stores logs, event data, and other IPMI information.
5. **Communication Interfaces**  
    Includes serial, LAN, local system interfaces, ICMB, and PCI management buses.
### Footprinting IPMI
IPMI uses **UDP port 623**.  
Devices that speak this protocol are called **BMCs (Baseboard Management Controllers)**.

A BMC is basically a small computer (usually ARM running Linux) that is connected directly to a server’s motherboard. Many servers already include a BMC, but you can also add one using a PCI card. Common examples are:
- **HP iLO**
- **Dell DRAC**
- **Supermicro IPMI**

If you gain access to a BMC, you can almost fully control the server as if you were physically touching it. You can:

- Turn it on/off
- Reboot it
- Monitor it
- Reinstall the OS

Because of this, compromising a BMC is almost the same as having physical access to the machine.

Most BMCs expose:

- A **web management interface**
- A **remote command interface** (SSH or Telnet)
- **Port 623/UDP** for IPMI