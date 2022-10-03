![open-memory-interface-omi-wht](pictures/open-memory-interface-omi-wht.png)



## **OMI Overview **

**OMI**, stands for Open Memory Interface. 

Check OMI web site at [https://openmemoryinterface.org/open-projects/](https://openmemoryinterface.org/open-projects/)

OMI is a highly tuned bus that was developed for near memory and is  easily migratable to emerging memory solutions (e.g., Storage Class  Memory). This serial coherent bus, a subset of [OpenCAPI](https://opencapi.org/) (3.1 version), was architected specifically for the interface between a processor and  Near Memory having absolute minimum latency with significant bandwidth  and capacity. OMI is the solution to our evolving industry’s demand for Near Memory as data centers evolve from compute centric to becoming  data centric.

Due to the smaller beachfront required by the OMI serial interface,  processors using OMI are able to support many more memory channels. For  example, IBM's Power10 is the first processor offering 1 Terabyte/s bandwidth on the memory side. The same bandwidth with external hardware is able to support up to 2 Petabytes of addressable memory.

Thanks to the very low latency, this OMI near-memory connection brings plenty of new memory disaggregation possibilities. "Memory inception" for example, allows a process to borrow host memory from another server. "Pool of memories" can now be built to optimize the sharing of the most expensive resource of our servers.

Using this CAPI/OpenCAPI technology associated with FPGAs has not only solved unbelievable bottlenecks, but has drastically decreased the power consumption of previous solutions.

This Documentation site presents an overview of host and device chips as well as an enablement guide.

- [Fire host](./blocs/fire) design example
- [Ice device](./blocs/ice) design example
- [Gemini device card](./blocs/gemini) design example
- [OMI Enablement Guide](./blocs/enablement)

 All the code and related materials are contributed to different Github repositories.

## REFERENCE DOCUMENTS and GUIDES

- DDIMM PMICs information available in the JEDEC Standard Document [JESD301-1A](https://www.jedec.org/standards-documents/docs/jesd301-1a)
- DDIMM EEPROM content "JEDEC Publication No. 106BE : STANDARD MANUFACTURER’S IDENTIFICATION CODE" [JEP106BE.pdf](https://www.jedec.org/standards-documents/docs/jep-106ab)

## FMC+ OMI ENABLEMENT GUIDE 

- [https://github.com/OpenCAPI/omi_enablement/](https://github.com/OpenCAPI/omi_enablement/) contains an example of host on a FMC+ board to evaluate DDMIMs modules

## HOST SIDE FPGA REFERENCE DESIGN

<https://github.com/OpenCAPI/omi_host_fire> contains a host FPGA reference design

## DEVICE SIDE FPGA REFERENCE DESIGN

<https://github.com/OpenCAPI/omi_device_ice> contains a device FPGA reference design

## DEVICE SIDE ASIC REFERENCE DESIGN

<https://github.com/OpenCAPI/omi_asic_device_reference_design> contains an ASIC device reference design




