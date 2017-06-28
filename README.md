# homebridge-cmdswitch2 [![npm version](https://badge.fury.io/js/homebridge-cmdswitch2.svg)](https://badge.fury.io/js/homebridge-cmdswitch2)
CMD Plugin for [HomeBridge](https://github.com/nfarina/homebridge) (API 2.0)

### How to use this plugin with Amazon Dash Buttons
In order to use this plugin with Amazon Dash Buttons you need to do the following things:
1. find the MAC adresse of your Amazon Dash Button(s)
   - via your router dashboard where the MAC adresse of your Dash Button should show up
   - via [node-dash-button](https://github.com/hortinstein/node-dash-button)
2. Assign your Dash a fixed IP adress (e.g. 192.168.0.230 - depends on your network)
3. leave on and off command empty - just state the line state_cmd (see Advanced Configuration and config-sample.json)
4. go to the HomeKit app of your choice and go to the automation tab
5. create a new automation and use "an Accessory is Controlled"
6. choose your Dash Button as Accessory and press active
7. choose your lamp, plug, etc. to be controlled.
8. DONE

### What this plugin does
Besides the functions intended by the main developer (see below) is plugin is ablet to Ping your Amazon Dash Button. This makes HomeKit know if a Dash Button has been pressed.

This plugin allows you to run Command Line Interface (CLI) commands via HomeKit. This means you can run a simple commands such as `ping`, `shutdown`, or `wakeonlan` just by telling Siri to do so. An example usage for this plugin would be to turn on your PS4 or HTPC, check if it’s on, and even shut it down when finished.

### How this plugin works
1. `on_cmd`: This is the command issued when the switch is turned ON.
2. `off_cmd`: This is the command issued when the switch is turned OFF.
3. `state_cmd`: This is the command issued when HomeBridge checks the state of the switch.
  1. If there is no error, HomeBridge is notified that the switch is ON.
  2. If there is an error, HomeBridge is notified that the switch is OFF.

### Things to know about this plugin
This plugin can only run CLI commands the same as you typing them yourself. In order to test if your `on_cmd`, `off_cmd`, or `state_cmd` are valid commands you need to run them from your CLI. Please keep in mind you will want to run these commands from the same user that runs (or owns) the HomeBridge service if different than your root user.

# Installation
1. Install homebridge using `npm install -g homebridge`.
2. Install this plugin using `npm install -g homebridge-cmdswitch2`.
3. Update your configuration file. See configuration sample below.

# Configuration
Edit your `config.json` accordingly. Configuration sample:
 ```
"platforms": [{
    "platform": "cmdSwitch2"
}]
```

### Advanced Configuration (Optional)
This step is not required. HomeBridge with API 2.0 can handle configurations in the HomeKit app.
 ```
"platforms": [{
    "platform": "cmdSwitch2",
    "name": "CMD Switch",
    "switches": [{
        "name" : "Dash Button Black",
        "state_cmd": "ping -c 2 192.168.0.230 | grep 'rtt min'",
        "polling": true,
        "interval": 3,
        
    }]
}]
```


| Fields           | Description                                           | Required |
|------------------|-------------------------------------------------------|----------|
| platform         | Must always be `cmdSwitch2`.                          | Yes      |
| name             | For logging purposes.                                 | No       |
| switches         | Array of switch config (multiple switches supported). | Yes      |
| \|- name\*       | Name of your device.                                  | Yes      |
| \|- on_cmd       | Command to turn on your device.                       | No       |
| \|- off_cmd      | Command to turn off your device.                      | No       |
| \|- state_cmd    | Command to detect an ON state of your device.         | No       |
| \|- polling      | State polling (Default false).                        | No       |
| \|- interval     | Polling interval in `s` (Default 1s).                 | No       |
| \|- manufacturer | Manufacturer of your device.                          | No       |
| \|- model        | Model of your device.                                 | No       |
| \|- serial       | Serial number of your device.                         | No       |
\*Changing the switch `name` in `config.json` will create a new switch instead of renaming the existing one in HomeKit. It's strongly recommended that you rename the switch using a HomeKit app only.
