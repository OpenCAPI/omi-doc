# C Enablement Software Description

## Architecture Details

OMI enablement platform requires software to configure/synchronize/exercise the host and the DDIMMs modules.

Initial development was performed using IBM's Cronus tool and can still be used if required.

To ease the discovery of OMI technology, a simple C code has been developed.

It can be run either on a external Raspberry pi, or any computer providing an I2C acces to the VCU128 card or on an internal Microblaze microprocessor (under development)

## Requirements

- "smbus2" librairies are required.
- General knowledge of C configuration on chosen hardware/OS as well as I2C skills are required.
- Source code is available [here]

[here]:https://github.com/OpenCAPI/omi_enablement/tree/main/c_code

## Compilation

For example on a raspberry pi : 

```
gcc *.c -o omirpi.out
```



## General Preparation and Settings

```
./omirpi.out --help
```

For a verbose display of any command, add the --log option just after omi.py

```
./omirpi.out --log yourcommand
```

With any command, the I2C bus must be provided with -b option.

If you are using a different I2C bus number, the number should be passed like this:

```
./omirpi.out anycommand -b 3
```

You can scan the I2C bus with the command:

```
./omirpi.out scan -b 3
```

!!! Warning "Warning !"
    Code is provided as is, if unexpected I2C lanes hangs occur, a complete reset of Fire might be required

## Adapter card Mux settings

To be able to access a ddimm from I2C, muxes need to be configured:

```
./omirpi.out initpath -d ddimm  # ddimm : I2C selected DDIMM . Can be a,b,none.
./omirpi.out initpath -d a
./omirpi.out initpath -d b
./omirpi.out initpath -d none
```

At any time, you can check the current path already set up with:

```
./omirpi.out checkpath
```

## Initialize device

Before using the following functions, use the following command to initialize the host,

without it, i2c errors will be encountered:

```
./omirpi.out init
```

## Host/device information

To print out information about a chip, just do:

```
./omirpi.out info -c chip # chip: host/device name. Can be either "fire" or "explorer" (or "ice")
./omirpi.out info -c fire
./omirpi.out info -c explorer
./omirpi.out info -c ice
```



## Reseting Device

### Change reset state of a DDIMM or multiple DDIMMs from fire:

- ```
  ./omirpi.out ddimmreset -d ddimms -s newresetstate
  ```

  ddimms: letters of ddimms selected (Examples: abcdsw, ab, a)

- newresetstate: **on** to activate reset mode, **off** to quit reset mode

### Example

```
  ./omirpi.out ddimmreset -d ab -s on
```

## Read a host/device register:

```
./omirpi.out read -c chip -r regaddr
```

-   chip: chip name. Can be either fire or explorer (or ice)

-   regaddr: register address given in hex

```
./omirpi.out read -c fire -r 0x100000000000004
./omirpi.out read -c explorer -r 0x20b080
./omirpi.out read -c explorer -r 0x8012811
```



## Write to a host/device register:

```
./omirpi.out write -c chip -r regaddr -d data
```

- chip: chip name. Can be either fire or explorer (or ice)
- regaddr: register address given in hex
- data: new value to write to the register, in hex

```
./omirpi.out write -c fire -r 0x100000000000004 -d 0x3f

./omirpi.out write -c explorer -r 0x8012811 -d 0x5000000006f
```



## Trigger training/synchronisation procedure:

```
./omirpi.out sync -d ddimms
```

-   ddimms: letters of ddimms to sync (Examples: ab, b, a)

  !! This function commutes the muxes automatically to sync the ddims provided.

  !! Make sure to wait for about 20s after powering/resetting devices before launchig this command, 

  !! otherwise it fails and needs to be re-executed.

```
./omirpi.out sync -d a
./omirpi.out sync -d b
./omirpi.out sync -d ab
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


