# micropython-simple-thermistor
read NTC thermistor temperature wired in a potential divider

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
