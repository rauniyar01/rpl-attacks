RPL Attack Framework
====================

Installation
------------

1. Clone this repository

 ```
 git clone ...
 ```

2. Install system requirements

 ```
 sudo apt-get install gcc-msp430 libcairo2-dev libffi-dev
 ```

3. Install Python requirements

 ```
 sudo pip install -r requirements.txt
 ```

 or

 ```
 sudo pip3 install -r requirements.txt
 ```

4. Install Cooja plugin RadioLoggerHeadless

 Refer to [this Git repository](https://github.com/cetic/cooja-radiologger-headless)


Configuration
-------------

Parameters :

- `contiki_folder`: the path to your contiki installation

>  [default: ~/contiki]

- `experiments_fodler`: the path to your experiments folder

>  [default: ~/Experiments]

These parameters can be tuned by editing ``~/.rpl-attacks.conf``. These are written in a section named "RPL Attacks Framework Configuration".

Example configuration file :

```
[RPL Attacks Framework Configuration]
contiki_folder = /opt/contiki
experiments_folder = ~/simulations
```


Commands
--------

Commands are used by typing ``fab:...``

- **`clean`**`:name`

> This will clean the simulation directory named 'name'.

- **`cooja`**`:name`

> This will open Cooja and load simulation named 'name'.

- **`launch`**`:name`

> This will start simulation in Cooja in no-GUI mode.

- **`make`**`:name[, make-target, external-rpl-library]`

> This will build all firmwares from ``root.c``, ``sensor.c`` and ``malicious.c`` templates with the specified target mote type. This can alternatively make the malicious mote with an external library by providing its path.

- **`new`**`:name[, number-of-nodes,
                      malicious-mote-type,
                      malicious-mote-config-constants,
                      simulation-duration,
                      simulation-title,
                      simulation-goal,
                      simulation-notes,
                      simulation-debug-mode]`

> This will create a simulation named 'name' with specified parameters.
> 
>  `malicious-mote-type`: root/sensor [default: sensor]
>  `malicious-mote-confic-constants`: every constant configurable in ContikiRPL implementation (e.g. `RPL_CONF_MIN_HOPRANKINC`), formatted as a dictionary [default: none]
> 
>  `simulation-duration`: in seconds [default: 300]
>  `simulation-title`, `simulation-goal`, `simulation-notes`: customized simulation-specific information for the Cooja simulation [default: none]
>  `simulation-debug-mode`: boolean [default: false]

- **`parse`**`:name`

> This will parse log files generated by Cooja to retrieve exchanged messages, power tracking measures, ...

- **`plot`**`:name`

> This will generate plots from parsed results.

- **`run_all`**`:simulation-campaign-json-file`

> This will generate a campaign of simulations from a JSON file formatted and execute the chain `clean`|`new`|`make`|`launch`|`parse`|`plot` for each simulation. See ``./templates/experiments.json`` for simulation campaign JSON format.


Simulation campaign
-------------------

Example JSON :

```
{
  "sinkhole": {
    "simulation": {
      "title": "Sinkhole Attack",
      "goal": "Show that the malicious node is creating another\n DODAG, preventing some motes from joining legitimate\n root's DODAG",
      "notes": "The malicious mote will use the same prefix as the\n root and prevent sensors from joining the legitimate\n DODAG",
      "number_motes": 10,
      "target": "z1",
      "duration": 120,
      "debug": true
    },
    "malicious": {
      "type": "sensor",
      "constants": {
        "RPL_CONF_MIN_HOPRANKINC": 128
      }
    }
  }
}
```
