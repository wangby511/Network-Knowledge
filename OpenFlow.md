## SDN

### OpenFlow

The SDN Pioneer

OpenFlow is a communications protocol that enables Software-Defined Networking (SDN). Before OpenFlow, network switches were "black boxes" where the control logic (deciding where data goes) and the forwarding hardware (actually moving the data) were inseparable.

Key Concepts:

* Control/Data Plane Separation - It moves the "intelligence" (Control Plane) out of the individual switch and into **a centralized SDN Controller**.

* Flow-Based Forwarding - Instead of looking only at destination IP addresses, OpenFlow allows switches to route traffic based on "Flows" (defined by a combination of MAC addresses, IPs, ports, etc.).

* The Flow Table - The controller pushes "Flow Entries" to the switch. If a packet matches a rule, the switch performs an action (Forward, Drop, or Modify).

### gPINS

Difference between P4 and gPINS:

gPINS (Google P4-Integrated Network Stack) is Google’s next-generation switch software stack. It is the "glue" that allows Google to use P4-programmable hardware within their existing SDN (Software Defined Networking) infrastructure. It represents a shift from "using a protocol" (OpenFlow) to "programming the hardware" (P4).

P4 (Programming Protocol-independent Packet Processors) is an open-source, domain-specific **language** used to tell network hardware exactly how to process data packets.

gPINS isn't just code; it’s a stack that includes **P4Runtime** (the control protocol), **gNMI/gNOI** (for management and operations), and **SAI** (Switch Abstraction Interface).

#### Architecture

P4-Based: Instead of a fixed set of rules, Google uses the P4 language to define exactly how the switch hardware should process packets. This provides "Forwarding Plane Programmability."

P4Runtime: This is the modern equivalent of OpenFlow. It is the API used by the remote controller to manage the P4-defined pipeline in real-time.

SAI (Switch Abstraction Interface): gPINS often sits on top of SAI, which allows Google to run the same software stack across different silicon (like Broadcom, Barefoot, or even their own custom chips).

Before gPINS, if Google bought a switch from Vendor A and a switch from Vendor B, they had to write different management code for each. gPINS provides a unified interface.

| Feature           	| OpenFlow                                      	| gPINS (P4-based)                                  	|
|-------------------	|-----------------------------------------------	|---------------------------------------------------	|
| Flexibility       	| Limited to predefined headers.                	| Fully programmable headers/logic.                 	|
| Control Interface 	| OpenFlow Protocol.                            	| P4Runtime + gNMI / gNOI.                          	|
| Hardware          	| Usually requires "OpenFlow-compatible" chips. 	| Works on any P4-capable or SAI-compliant silicon. 	|