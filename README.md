# MT175 SML to Influxdb2
Reading SML-Messages from ISKRA MT175 and write them to influxdb2.

This script es based on Alexander Kabza's work documented [here](http://www.kabza.de/MyHome/SmartMeter/SmartMeter.html). 

# Requirements

* Raspberry Pi (actually everything which runs Python and has a serial interface should work)
* bitShake [SmartMeterReader UART](https://www.ebay.de/itm/125077162292?mkcid=16&mkevt=1&mkrid=707-127634-2357-0&ssspo=fb9jjuttsbe&sssrc=2047675&ssuid=&widget_ver=artemis&media=COPY) (others should work as well)
* unlocked ISKRA MT175 Smart-Meter (ask your Messtellenbetreiber for a PIN to unlock) 

# Setup

The easiest way to get the script running is to use [`pipenv`](https://pipenv.pypa.io/en/latest/installation/). 

```bash
cd MT175_SML_Reader
pipenv sync
```
This installs all required python modules with the exact version specified in `Pipfile.lock` into the default location outside your project directory. 

> By default, the virtualenv is stored globally with the name of the project’s root directory plus the hash of the full path to the project’s root (e.g., my_project-a3de50).
>
> -- <cite>[pipenv docs](https://pipenv.pypa.io/en/latest/installation/#virtualenv-mapping-caveat)</cite>

The packages are locked to versions and hashes availble for plattform `aarch64`.

 # going live

 The script reads a couple of environment variables, which contain Influx- and Serial-Connection details. 

 ```bash
 #!/bin/sh

export INFLUX_TOKEN="your token goes here"
export INFLUX_ORG="yourorg"
export INFLUX_BUCKET="yourbucket"
export INFLUX_HOST="http://192.168.178.44:8086"
export SERIAL_PORT="/dev/ttyS0"
```
Use `source ./setenv.sh` to make those variables available in your shell. 

Then start the script and send it to background with 

```bash
nohup pipenv run python3 smlreader.py &
```

Then check your influxdb for new data. 