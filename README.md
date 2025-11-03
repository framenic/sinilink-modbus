esphome configuration for sinilink power supply modules.

The sinilink power supplies (XY5008, XYH3680, ...) are low cost modules that work quite well for their price. They have a minimal interface with just an ON/OFF button and a rotary encoder+push button, an IR remote control, and they can also be controlled via wifi using an additional module (XY-WFPOW), which is connected to the XY5008 using the UART lines. The wifi connection works, unfortunaley, through obscure cloud servers and requires a proprietary app to be installed on your mobile.

This is a yaml configuration to produce an esphome image for esp8285/esp8266 based modules. It can be used with the original XY-WFPOW modules (based on esp8285) or any esp8266 module with 1M flash and hardware UART TX/RX lines (GPIO1/GPIO3). It connects to the XY5008 module using modbus protocol over UART and can be used for remote control of the power supply (home assistant or direct web interface). When connected to wifi, the IP address of the module is displayed on the UI.

The yaml code provide 3 additional functions to the module:

- under current cutoff: if set to a vaule different from 0 will turn off the power supply when the output current goes uder the vaule. This can be used when using the power supply as a battery charger, to stop the charge of a battery after the common constant-current constant-voltage phases.
- output relay switch on GPIO4: when used for charging batteries, the module will absorb about 5mA from the battery when the output is turned off (i.e. after charging). To avoid this aborption a relay module can be used to disconect the output, controlled by GPIO4.
- output relay power off when VIN < 17V. This value is hardcoded (can be changed before compilation) and is used for shutting down the power supply when used as a battery charger. In case of main power failure (VIN=0), the XY5008 module will stay on drawing current from the battery (about 150mA) and discharging it. All batteries with voltage lower than 17V will trigger the automation.

Note: the XY5008 module does not have any protection against reverse polarity connection if used as a battery charger, pay extra attention before connecting a battery to its outputs.

![xy5008](https://github.com/framenic/sinilink-modbus/assets/30783647/dd57f4ea-96eb-42da-8486-eefb3178d858)

![xy-wfpow](https://github.com/framenic/sinilink-modbus/assets/30783647/1149eb05-5382-4c24-ba2d-4d220832223f)

![web_interface](https://github.com/framenic/sinilink-modbus/assets/30783647/729f6a2f-bd00-46ed-bdfd-bd4ca99f4323)

![homeassistant_interface](https://github.com/framenic/sinilink-modbus/assets/30783647/93cef264-32a9-4b22-b79c-bc7a67fdc7b3)

![output realy](https://github.com/framenic/sinilink-modbus/assets/30783647/7c571b12-1c40-4857-bd86-30a9a6905e87)

