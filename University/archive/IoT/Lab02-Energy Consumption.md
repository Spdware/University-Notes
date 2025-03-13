Usually in IoT devices you cannot receive power from a network, but you need to use a battery as they are deployed in remote and inaccessible environment to reach for electricity cables. So you want to consider all the techniques to save energies. Other solutions, less reliable, are to use kinetic, solar or Radio waves power to recharge our device.
# ESP32 Sleep Mode
ESP32 comes with 5 different Power modes: modem-sleep, light-sleep, deep-sleep, hibernation, power-off.
## ESP32 Active Mode
All peripherals are active, radio, bluetooth and Wi-Fi module are active and the processing core are active. 
## Modem Sleep Mode
Only turn on some connection at fixed intervals
## Light Sleep Mode
Peripherals are disables and is like an interrupt, when you wake up you resume the execution from the point you have gone sleeping, main core still runs.
## Deep Sleep Mode
You have very small power consumption
# ESP32 WiFi Transmission Power
We can set the transmission power. 