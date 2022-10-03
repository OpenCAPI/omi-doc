# Python Enablement Software Description

## Architecture Details

OMI enablement platform requires software to configure/synchronize/exercise the host and the DDIMMs modules.

Initial development was performed using IBM's Cronus tool and can still be used if required.

To ease the discovery of OMI technology, a simple python code has been developed.

It can be run either on a external Raspberry pi, or any computer providing an I2C acces to the VCU128 card or on an internal Microblaze microprocessor (under development)

## Requirements

- "click" and "smbus2" librairies are required.
- General knowledge of python configuration on chosen hardware/OS as well as I2C skills are required.
- Source code is available [here]

[here]:https://github.com/OpenCAPI/omi_enablement/tree/main/python

## General Preparation and Settings

This application is run from CLI (Command Line Interface) with Python3:

```
python3 omi.py --help
```

For a verbose display of any command, add the --log option just after omi.py

```
python3 omi.py --log yourcommand
```

With any command, the I2C bus can be provided with -b option. **By default I2C bus is set to 3** if not given.

If you are using a different I2C bus number, the number should be passed like this:

```
python3 omi.py anycommand -b 1
```

You can scan the I2C bus with the command:

```
python3 omi.py scan -b 3
```

!!! Warning "Warning !"
    Code is provided as is, if unexpected I2C lanes hangs occur, a complete reset of Fire might be required

## Adapter card Mux settings

To be able to access a ddimm from I2C, muxes need to be configured:

```
python3 omi.py initpath -d ddimm  # ddimm : I2C selected DDIMM . Can be a,b,none.
python3 omi.py initpath -d a
python3 omi.py initpath -d b
python3 omi.py initpath -d none
```

At any time, you can check the current path already set up with:

```
python3 omi.py checkpath
```

## Initialize device

Before using the following functions, use the following command to initialize the host,

without it, i2c errors will be encountered:

```
python3 omi.py init
```

## Host/device information

To print out information about a chip, just do:

```
python3 omi.py info -c chip # chip: host/device name. Can be either "fire" or "explorer" (or "ice")
python3 omi.py info -c fire
python3 omi.py info -c explorer
python3 omi.py info -c ice
```



## Reseting Device

### Change reset state of a DDIMM or multiple DDIMMs from fire:

- ```
  python3 omi.py ddimmreset -d ddimms -s newresetstate
  ```

  ddimms: letters of ddimms selected (Examples: abcdsw, ab, a)

- newresetstate: **on** to activate reset mode, **off** to quit reset mode

### Example

```
    python3 omi.py ddimmreset -d ab -s on
```

## Read a host/device register:

```
python3 omi.py read -c chip -r regaddr
```

-   chip: chip name. Can be either fire or explorer (or ice)

-   regaddr: register address given in hex

```
python3 omi.py read -c fire -r 0x100000000000004
python3 omi.py read -c explorer -r 0x20b080
python3 omi.py read -c explorer -r 0x8012811
```



## Write to a host/device register:

```
python3 omi.py write -c chip -r regaddr -d data
```

- chip: chip name. Can be either fire or explorer (or ice)
- regaddr: register address given in hex
- data: new value to write to the register, in hex

```
python3 omi.py write -c fire -r 0x100000000000004 -d 0x3f

python3 omi.py write -c explorer -r 0x8012811 -d 0x5000000006f
```



## Trigger training/synchronisation procedure:

```
  python3 omi.py sync -d ddimms
```

-   ddimms: letters of ddimms to sync (Examples: ab, b, a)

  !! This function commutes the muxes automatically to sync the ddims provided.

  !! Make sure to wait for about 20s after powering/resetting devices before launchig this command, 

  !! otherwise it fails and needs to be re-executed.

```
python3 omi.py sync -d a
python3 omi.py sync -d b
python3 omi.py sync -d ab
```

## Trigger DDIMM Configuration procedure:

```
  python3 omi.py ddimmcfg -d ddimms
```

-   ddimms: letters of ddimms to sync (Examples: ab, b, a)

This function will trigger a configuration process with Fire commands (Explorer will be accessed through In-band commands)
No need to select the MUX path, as only Fire receives I2C commands.

```
python3 omi.py ddimmcfg -d a
python3 omi.py ddimmcfg -d b
python3 omi.py ddimmcfg -d ab
```



## To check DDIMM training state:

```
  python3 omi.py checksync -d ddimms
```

* ddimms: letters of ddimms to sync check (Examples: ab, b, a)

```
python3 omi.py checksync -d a
python3 omi.py checksync -d b
python3 omi.py checksync -d ab
```



## Examples of Usage

```
python3 omi.py initpath -d a
python3 omi.py init
python3 omi.py sync -d a
python3 omi.py info -c explorer
python3 omi.py checkpath
pi@raspberrypi:~/python $ python3 omi.py initpath -d a
pi@raspberrypi:~/python $ python3 omi.py init
pi@raspberrypi:~/python $ python3 omi.py sync -d a
----------         : Explorer Initialisation    ------------
Explorer Firmware API version: 0x0f Ready
Sync DDIMMA...
----------        Explorer OMI Training Sequence ------------
----------        Fire     OMI Training Sequence ------------
DDIMMA sync Reg: 0x8000000000
Training successfully done.
pi@raspberrypi:~/python $ python3 omi.py ddimmcfg -d a
Configuring DDIMMA...
DDIMMA Configuration
Memory Size : 64GB
Vendor      : SMART
Memory Size : 64GB
Vendor      : SMART
pi@raspberrypi:~/python $ python3 omi.py info -c explorer
FW number of images: 0x2
Partition ID: 0x41
Major (Boot Partion A): 0x8
Minor (Boot Partion A): 0x0
Build patch (Boot Partion A): 0x0
Build number (Boot Partion A): 0x6792a
Build date (Boot Partion A): 0x2222021
Major (Boot Partion B): 0x8
Minor (Boot Partion B): 0x0
Build patch (Boot Partion B): 0x0
Build number (Boot Partion B): 0x6792a
Build date (Boot Partion B): 0x2222021
RAM size (in bytes): 0x40000
Chip version: 0x20600d2
SPI flash ID: 0xbb98
SPI flash sector size: 0x10000
SPI flash size: 0x1000000
Error buffer size: 0x1000
Image index: 0x0
ECID: 0x1000000000000721a6d03b4f8759f0d4ed89300
Entreprise Mode Status: 0x796
Card ID: 0x3e00008
EEPROM data: Reading 0x40c0a8529000800600003080b8000
Memory Size : 32GB
```
