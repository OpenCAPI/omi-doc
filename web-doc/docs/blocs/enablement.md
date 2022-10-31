# OMI ENABLEMENT Guide

## INTRODUCTION

This guide will cover the steps to evaluate OMI using FIRE and EXPLORER designs.

First thing is to setup hardware and software environments.

Eventually go into some details of the registers and memory space accesses.

Finally use the system to get familiar with the OMI technology

## SETUP

This [enablement setup](./enablement_setup.md) page will present hardware and software examples to allow proper evaluation of OMI.

This step should allow you to read FIRE's ID register:

It contains the git revision of the hardware build and few other information:

`python3 omi.py read -c fire -r 0x100000000000000`
`Rd Fire Addr 0x100000000000000 : 0x000000003e29c7d2`       (This an example of git level)

## OMI synchronisation

Once FIRE is accessible and DDIM are powered up, we need to synchronize the OMI links.

After power supplies are set to ON and FIRE being reset cycled.

This is simply achieved with the following python commands for DDIMMA, A or both:

```
python3 omi.py initpath -d a; python3 omi.py init; python3 omi.py sync -d a
python3 omi.py initpath -d b; python3 omi.py init; python3 omi.py sync -d b
python3 omi.py initpath -d a; python3 omi.py init; python3 omi.py initpath -d b; python3 omi.py init; python3 omi.py sync -d ab
```

## OMI CONFIGURATION AND TESTS

Once OMI links are set, the latest step consists in configuring the EXPLORER chip to provide access to its local DDIMM memory.

```
python3 omi.py ddimmcfg -d a
```

Once this is done, FBIST and C3S tests can be run.

### FBIST TESTS

The following routines allow users to experiment with embedded fbist generators to write/read data into DDIMMs and evaluate performances.

!!! Note    " The reference design is aimed at mainly providing mecanisms, it has not been optimized for performance"

```
$ python3 omi.py fbistcfg -d a

======================================================

-------------------Start FBIST procedure -------------

#=============================
 Commands configuration for DDIMM located in port a

>> Number of Engines to enable ? (Type 1 to 8): 1
>> Number of Bytes to test access? (Type 2 for 64B and 4 for 128B): 2
>> Number of cycles between flits (from FF to 0, typical is 0): FF
>>                                                    
>> FBIST_POOL_0_ENGINE_x - 0x02= 64B WR + 0x01=@Incremental address
>> Spacing of 255 cycles between Flits
>> Data pattern is 0x0 : Data Equals Address
>>                                                    
>>  -- 6 secs run - please wait --

-------------------------

Errors     :  no errors found 
Run Time   : 6.03 sec
Latency E0 : 0.22 secs   <<== only one generator was chosen
Latency E1 : 0.00 secs
Latency E2 : 0.00 secs
Latency E3 : 0.00 secs
Latency E4 : 0.00 secs
Latency E5 : 0.00 secs
Latency E6 : 0.00 secs
Latency E7 : 0.00 secs
Sum of lat : 0.22 secs
Total WR   : 980674
nb 32B wr  : 1961348
Access is  : WRITE 64 Bytes
freq       : 333 MHz
Avg Wr lat : 220.22 ns

Throughput : 0.01 GBps

Type 'q' to quit or any key to continue with Read fbist procedure
```



### C3S TESTS

... working on it ...

## Firmware checking

### Check or update firmware level 

Using info option will provide the revision in hexa and decimal format.

```
python3 omi.py info -c exp
...
Build number (Boot Partition A) : 0x6c92a - 444714
...
```

Get latest firmware there:[https://github.com/open-power/ocmb-explorer-fw/releases](https://github.com/open-power/ocmb-explorer-fw/releases)

[https://github.com/open-power/ocmb-explorer-fw/releases]: (https://github.com/open-power/ocmb-explorer-fw/releases)

For example if 444714 is the latest fimware:

1. unzip the FW444714BinaryOnly.zip
2. `cp signed_app_fw.mem <your python directory>/CL444714.bin`
3. edit firmware_update.py replacing "<release\>.bin" by CL444714.bin
4. then use this command  to update the fimware: `python3 firmware_update.py`
5. at the end, check with ``python3 omi.py info -c exp` that the firmware partition B has the required level, ready to become the used firmware after next resets.
6. Reset the card and allow the firmware to be updated
7. finally check the "info" to confirm proper update.



Go back to the [enablement](./enablement.md) page