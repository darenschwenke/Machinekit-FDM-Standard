### Abbreviations
| Abbreviation      | Name    |
| :----: | :-----: |
| FDM | Fused Deposition Modeling |
| HBP | Heated Build Platform |
| HBC | Heated Build Chamber |
| En | Extruder n |
| Fn | Fan n |
| Ln | Light n |
| Sn | Servo n |
| Pn | Probe n |

### Analog Output
| #      | Name    |
| :----: | :-----: |
| 0 | HBP target temperature |
| 1 | HBC target temperature |
| 2 - 11 | E0-E9 target temperature |
| 12 - 21 | F0-F9 speed |
| 22 - 25 | S0-S3 pulse duration |
| 26 | L0 red value |
| 27 | L0 green value |
| 28 | L0 blue value |
| 29 | L0 white value |
| 30 - 37 | L1-L2 RGBW values |
| 38 | Probe selection (for homing) |
| 39 | Buzzer frequency |
| 40 | Buzzer duration |
| 41 | VE line area |
| 42 | VE jog velocity |
| 43 | VE jog length |
| 44 | filament diameter |

### Analog Input
| #      | Name    |
| :----: | :-----: |
| 0 | HBP measured temperature |
| 1 | HBC measured temperature |
| 2 - 11 | E0-E9 measured temperature |
| 12 - 21 | F0-F9 speed |
| 22 - 25 | S0-S3 pulse duration |
| 26 | L0 red value |
| 27 | L0 green value |
| 28 | L0 blue value |
| 29 | L0 white value |
| 30 - 37 | L1-L2 RGBW values |
| 38 | unused |
| 39 | Buzzer frequency |
| 40 | Buzzer duration |
| 41 | VE line cross section |
| 42 | VE line width |
| 43 | VE line height |
| 44 | filament diameter |
| 45 | VE jog velocity |
| 46 | VE jog length |

### Digital Output
| #      | Name    |
| :----: | :-----: |
| 0 | Probe enable |
| 1 | Extruder enable |
| 2 | Buzzer trigger |
| 13 | VE jog trigger output |

### Digital Input
| #      | Name    |
| :----: | :-----: |
| 0 | HBP temperature in range |
| 1 | HBC temperature in range |
| 2 - 11 | E0-E9 temperature in range |
| 12 | VE retract feedback |
| 13 | VE jog trigger input |
| 14 | VE jog feedback |

## HAL Remote Components


### HBP, HBC and Extruders
Heated build platform, heated build chamber and extruders have the same interface. The name of the remote components are `fdm-hbp`, `fdm-hbc` and `fdm-e<n>`.

Example interface:

    newcomp fdm-e0 timer=100
    newpin  fdm-e0 fdm-e0.temp.meas      float in
    newpin  fdm-e0 fdm-e0.temp.set       float io
    newpin  fdm-e0 fdm-e0.temp.standby   float in
    newpin  fdm-e0 fdm-e0.temp.limit.min float in
    newpin  fdm-e0 fdm-e0.temp.limit.max float in
    newpin  fdm-e0 fdm-e0.temp.in-range  bit   in
    newpin  fdm-e0 fdm-e0.error          bit   in
    newpin  fdm-e0 fdm-e0.active         bit   in
    ready   fdm-e0
    
### Fans
Fans and other PWM controlled devices have a very simple interface:

    newcomp fdm-f0 timer=100
    newpin fdm-f0 fdm-f0.set float io
    ready fdm-f0

### Lights

    newcomp fdm-l0 timer=100
    newpin fdm-l0 fdm-l0.r float io
    newpin fdm-l0 fdm-l0.g float io
    newpin fdm-l0 fdm-l0.b float io
    newpin fdm-l0 fdm-l0.w float io
    ready  fdm-l0
