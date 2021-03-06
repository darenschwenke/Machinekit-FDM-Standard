# Machinekit Flavor GCode

## GCodes

### [G22](./subroutines/g22.ngc): Retract Extruder
Retracts the currently active extruder.

### [G23](./subroutines/g23.ngc): Unretract Extruder
Unetracts the currently active extruder.

### [G28](./subroutines/g28.ngc): Move to Origin
Example: `G28`

This code exists for compatibility reasons with RepRap flavor GCode. Machinekit does not support homing from GCode as homing needs to be triggered from the user interface. This code causes the FDM machine to move back to its X, Y and Z zero coordinates and resets the extruder axis. 

If you add coordinates, then just the axes with coordinates specified will be zeroed. Thus `G28 X0 Y72.3` will zero the X and Y axes, but not Z. The actual coordinate values are ignored.

### [G29](./subroutines/g29.ngc): Detailed Z-Probe
Example: `G29`

Probes the bed at a specified number of point points (usually 3-4).

### [G29.1](./subroutines/g29_1.ngc): Set Z probe head offset
Example: `G29.1 X30 Y20 Z0.5`

Set the offset of the Z probe head. The offset will be subtracted from all probe moves.

### [G29.2](./subroutines/g29_2.ngc): Set Z probe head offset calculated from toolhead position
Example: `G29.2 Z0.0`

Set the offset of the Z probe head. The offset will be subtracted from all probe moves. The calculated value is derived from the distance of the toolhead from the current axis zero point.

The user would typically place the toolhead at the zero point of the axis and issue the G29.2 command.

### [G30](./subroutines/g30.ngc): Single Z Probe
Example: `G30 X10 Y0`

In its simplest form probes bed at current XY location.

If X, or Y values are specified (e.g. `G30 X20 Y50`) then those values are used instead of the machine's current coordinates. The combination of these options allows for the machine to be moved to points using G1 commands, and then probe the bed, or for the user to position the nozzle interactively and use those coordinates. The user can also record those values and place them in a setup GCode file for automatic execution.

## MCodes
Machinekit supports a number of FDM specific MCodes inspired by the [RepRap MCodes](http://reprap.org/wiki/G-code). These codes are implemented using remapping. If you are interested in developing your own Machinekit based 3D printer take a look at the [remap file](remap.ini).

### [M104](./subroutines/m104.ngc): Set Extruder Temperature
Example: `M104 P190`

Set the temperature of the current extruder to 190 °C and return control to the host immediately (i.e. before that temperature has been reached by the extruder). 

#### Multiple Exruders
M104 can be additionally used to handle all devices using a temperature sensor. It supports the additional T parameter, which is a zero-based index into the list of sensors. For devices without a temp sensor, see M106.

Example: `M104 T1 P100`

Set the temperature of the device attached to the second temperature sensor to 100 °C.

### [M106](./subroutines/m106.ngc): Fan On
Example: `M106 P127`

Turn on the cooling fan at half speed.

Mandatory parameter 'S' declares the PWM value (0-255). M106 S0 turns the fan off.

### [M107](./subroutines/m107.ngc): Fan Off
Example: `M107`

Turn on the cooling fan off.

Deprecated. Use `M106 P0` instead. (But used by Slic3r)

#### Additional Fans/Devices
Additionally to the above, Machinekit uses M106 to control general devices. It supports the additional T parameter, which is an zero-based index into the list of fans/devices.

Example: `M106 T2 P255`

Turn on device #3 at full speed/wattage.

#### Sync with Motion
By default the M106 command will be executed with the next movement. This default behavior is used since the fan speed is constantly switched during execution and an unsynchronized movement would break look-ahead and path blending. For the purpose of executing a command immediately the M106 supports the additional I parameter.

Example: `M106 I1 P127`

Turn on the cooling fan at half speed immediately. `I0` equals to not immediate, all other values evaluate to immediate if I is specified.

### [M109](./subroutines/m109.ngc): Set Extruder Temperature and Wait
Example: `M109 P185`

Set extruder heater temperature in degrees celsius and wait for this temperature to be achieved.

#### Multiple Exruders
Similar to M104 this command supports the additional T parameter for specifying the extruder.

Example: `M109 T1 P100`

Set the temperature of the device attached to the second temperature sensor to 100 °C and wait for the temperature to be reached.

### [M140](./subroutines/m140.ngc): Bed Temperature (Fast)
Example: `M140 P55`

Set the temperature of the build bed to 55 °C and return control to the host immediately (i.e. before that temperature has been reached by the bed).

### [M141](./subroutines/m141.ngc): Chamber Temperature (Fast)
Example: `M141 P30`

Set the temperature of the chamber to 30 °C and return control to the host immediately (i.e. before that temperature has been reached by the chamber).

### [M190](./subroutines/m190.ngc): Wait for bed temperature to reach target temp
Example: `M190 P60`

Set the temperature of the build bed to 60 °C and wait for the temperature to be reached.

### [M191](./subroutines/m191.ngc): Wait for chamber temperature to reach target temp
Example: `M191 P60`

Set the temperature of the build chamber to 60 °C and wait for the temperature to be reached.

### [M200](./subroutines/m200.ngc): Set filament diameter
Example: `M200 D1.75`

`M200 Dm.mmm` sets the filament diameter to m.mmm millimeters. It is used with
'volumetric calibration' and G-code generated for an ideal 1.128mm diameter
filament, which has a volume of 1mm^3 per millimeter. The intention is to be
able to generate filament-independent g-code.

### [M226](./subroutines/m226.ngc): Gcode Initiated Pause
Example: `M226`

Initiates a pause in the same way as if the pause button is pressed. That is, program execution is stopped and the printer waits for user interaction. This matches the behaviour of M1 in the [NIST RS274NGC G-code standard](http://www.nist.gov/manuscript-publication-search.cfm?pub_id=823374).

### [M280](./subroutines/m280.ngc): Set servo position
Example: `M280 T0 P1500`

Set servo position absolute. T: servo index, P: angle or microseconds

### [M300](./subroutines/m300.ngc): Play beep sound
Usage: `M300 Q<frequency Hz> P<duration ms>`

Example: `M300 Q300 P1000`

Play beep sound, use to notify important events like the end of printing. The P parameter is optional and may not be supported by all electronics that implement the buzzer.

### [M400](./subroutines/m400.ngc): Wait for current moves to finish
Example: `M400`

Finishes all current moves and and thus clears the buffer. That's identical to `G4 P0`.

### [M420](./subroutines/m420.ngc): Set RGBW Colors as PWM
Usage: `M420 T<LED index (0-2)> R<Red PWM (0-1)> E<Green PWM (0-1)> D<Blue PWM (0-1)> P<White PWM (0-1)>`

Example: `M420 R1.0 E1.0 D1.0 P1.0`

Set the color of your RGBW LEDs that are connected to PWM-enabled pins. Note, the Green color is controlled by the E value instead of the G value due to the G code being a primary code that cannot be overridden. The optional T parameter specifies the index of the LEDs to set (default 0).

### [M700](./subroutines/m700.ngc): Set line cross section
Example: `M700 P0.061`

Sets the cross section for a line to extrude in velocity extrusion mode. When
the extruder is enabled and movement is executed the amount of extruded
filament will be calculated to match the specified line cross section.

## RepRap Alternatives
Some RepRap GCodes can not be implemented but easily replaced by native Machinekit GCodes.

### M206: Set home offset
Replacement: `G10 L2 P1 X10.0 Y10.0 Z-0.4`

The values specified are added to the endstop position when the axes are referenced. The same can be achieved with a G92 right after homing (G28, G161).

### M306: Set home offset calculated from toolhead position
Replacement: `G10 L20 P1 X10.0 Y10.0 Z-0.4`

The values specified are added to the calculated end stop position when the axes are referenced. The calculated value is derived from the distance of the toolhead from the current axis zero point.

The user would typically place the toolhead at the zero point of the axis and issue the M306 command.
