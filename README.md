# joycontrol
Emulate Nintendo Switch Controllers over Bluetooth.

Differences from the main fork: added macro specifically to farm Watt on Pokemon Sword and Shield.

Tested on Ubuntu 19.10, and with Raspberry Pi 3B+ and 4B Raspbian GNU/Linux 10 (buster)

## Features
Emulation of JOYCON_R, JOYCON_L and PRO_CONTROLLER. Able to send:
- button commands
- stick state
- ~~nfc data~~ (removed, see [#80](https://github.com/mart1nro/joycontrol/issues/80))

## Installation
Arch Linux Derivatives: Install the `hidapi` and `bluez-utils-compat`(AUR) packages

On Ubuntu/Raspbian:
```bash
sudo apt install python3 python3-pip python3-dbus libhidapi-hidraw0
```

```bash
git clone https://github.com/atvacs/joycontrol.git
```

```bash
cd joycontrol
```
(Note: Controller script needs super user rights, so python packages must be installed as root)
```bash
sudo pip3 install .
```

If you don't need to connect anything else to the device, disable the bluez "input" plugin, see [#8](https://github.com/mart1nro/joycontrol/issues/8). To do that, access the file `/lib/systemd/system/bluetooth.service`
```bash
sudo nano /lib/systemd/system/bluetooth.service
```
And change the following line
```
ExecStart=/usr/lib/bluetooth/bluetoothd
```
to
```
ExecStart=/usr/lib/bluetooth/bluetoothd --noplugin=input
```
and restart Bluetooth with

```bash
sudo systemctl daemon-reload
sudo systemctl restart bluetooth
```

When you run the script, you will probably get errors saying that a certain module can't be found. If so, just type the following command, replacing "MODULE_NAME" with the actual name of the module that is missing:
```bash
sudo pip3 install MODULE_NAME
```

## Command line interface example
- Run the script
```bash
sudo python3 run_controller_cli.py PRO_CONTROLLER
```
This will create a PRO_CONTROLLER instance waiting for the Switch to connect.

- Open the "Change Grip/Order" menu of the Switch

The Switch only pairs with new controllers in the "Change Grip/Order" menu.

Note: If you already connected an emulated controller once, you can use the reconnect option of the script (-r "\<Switch Bluetooth Mac address>").
This does not require the "Change Grip/Order" menu to be opened. You can find out the Bluetooth MAC address of your Switch by looking at the first lines that are printed after connecting. 

- After connecting, a command line interface is opened. Note: Press \<enter> if you don't see a prompt.

Call "help" to see a list of available commands.

- If you call "test_buttons", the emulated controller automatically navigates to the "Test Controller Buttons" menu. 


## Issues
- Some bluetooth adapters seem to cause disconnects for reasons unknown, try to use an usb adapter instead 
- Incompatibility with Bluetooth "input" plugin requires a bluetooth restart, see [#8](https://github.com/mart1nro/joycontrol/issues/8)
- It seems like the Switch is slower processing incoming messages while in the "Change Grip/Order" menu.
  This causes flooding of packets and makes pairing somewhat inconsistent.
  Not sure yet what exactly a real controller does to prevent that.
  A workaround is to use the reconnect option after a controller was paired once, so that
  opening of the "Change Grip/Order" menu is not required.
- ...


## Resources

[Nintendo_Switch_Reverse_Engineering](https://github.com/dekuNukem/Nintendo_Switch_Reverse_Engineering)

[console_pairing_session](https://github.com/timmeh87/switchnotes/blob/master/console_pairing_session)
