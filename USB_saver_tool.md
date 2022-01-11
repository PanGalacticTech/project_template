# Notes for USB Saver Tool


Started these notes as an example, may wish to go back




### Example Project Brief - [USB Saver Tool]

> The client requires a PCB to protect laptops and personal devices from damage
> while programming or powering USB enabled embedded systems projects during
> development & prototyping.




## [1.3]Example High Level Requirements Capture - [USB Saver Tool]


> HL.1. Device must protect Client USB from direct short between VCC & GND pins                                            <br>
> HL.2. Device must protect Client USB from current draw >0.5A, as common for older devices.                               <br>
> HL.3. Device must protect Client USB from voltages >5v applied to VCC and Data Pins.                                     <br>
> HL.4. Device should prevent voltages >1v being applied to the client USB GND pin.                                        <br>
> HL.5. Device should operate with <0.2A quiessent current, in order to maximise power available from client USB for embedded project.   <br>
> 

## [2.3]Example Low Level Requirements Capture - [USB Saver Tool]

#### [2.3.1]Hardware Requirements
>
> HW.1. 500mA resettable fuse in series with USB 5v pin, to limit current draw in case of short circuit                      - Trace to: (HL.1, HL.2)   <br>
> HW.2. 5.1v Zener diodes reverse biased between GND and USB Data+, and GND & USB Data, shunts (V > 5.1v) to GND             - Trace to: (HL.3)         <br>
> HW.3. P-Channel MOSFET & voltage sensing circuit on USB Vcc, to disconnect if (V > 5.5v)[^2].                              - Trace to: (HL.3)         <br>
> HW.4. Voltage Divider in voltage sensing circuit current consumption estimated at ~0.3mA[^3].                              - Trace to: (HL.4)         <br>                     




#### [2.3.2]Software Requirements

> SW.1. - *Example ID Number this project does not have any software requirements*
