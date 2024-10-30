# micropython-simple-thermistor
read NTC thermistor temperature wired in a potential divider

## WILL NOT WORK WITH

* ESP8266 (limited 1 V max ADC)
* W600 (no ADC!)

## WILL NOT IMPLEMENT

* High-side thermistor
* °F conversion

## TODO

* some docs would be nice
* implement:
    * Beta calculation from known second temperature and resistance (T₁, R₁): `β(T₀/T₁) = ln(R₁/R₀) / (1/T₁ - 1/T₀)`
    * Steinhart-Hart three-term calculation
* platform testing:
    * rp2
    * EPS32
    * STM32/pyboard
    * SAMD
    * external ADC?
* thermistor type testing
    * MF52
    * Adafruit [10K Precision Epoxy Thermistor - 3950 NTC](https://www.adafruit.com/product/372)
    * 100 kΩ glass bead thermistor (aka 3d printer thermistor)
    * Vishay NTCLE100E3

## Sample Code

```python3
# MicroPython, Raspberry Pi Pico
# pylint: disable=E0401
# -*- coding: utf-8 -*-

import machine
import time
from simple_thermistor import Thermistor


# setup for KY-013-alike thermistor on GPIO 26
t = Thermistor(
    adc=machine.ADC(
        26
    ),  # ADC object with read_u16() method returning 0–65535
    beta=3950,
    r0=10000,  # thermistor nominal resistance of 10 kΩ at 25 °C
    t0=Thermistor.fromC(25),  # everything is Kelvin internally
    resistor=10000,  # high side resistor of 10 kΩ
)
while True:
    print(
        "%6.1f °C / %.1f Ω"
        % (Thermistor.toC(t.temperature()), t.resistance())
    )
    time.sleep(5)
```
