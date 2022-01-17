# [2]  System Specification - [Project Title]

_This form is intended to assist in optioneering to derive low level hardware & software specification from the ***Validated***  
High Level Requirements Capture Form [[1]requirements_capture.md](https://github.com/PanGalacticTech/project_template/blob/main/%5B1%5Drequirements_capture.md).
Its scope can be adapted to suit projects of varying complexity_ <br>
_______________________________________________________________________________________________________________________________________________________
## [2.1] Design Tradeoffs & Optioneering
_Space to work through different options before deciding on specific low level requirements & system specification for hardware & software_

### Preamble
_Introduction to the design approach if required_ <br>

### Optioneering Diagrams
_Flowcharts can be used to explore the relationships between different options, and display graphically how making specific design decisions
will affect the other available options._

![image](https://user-images.githubusercontent.com/53580358/149507075-3be11e9f-9cd9-4e3a-bcb2-2ddf4bbdd5c4.png)
_Flowchart showing the relationship between possible system components_


### Optioneering Table
_This is an example of the options that may wish to be explored for this project, however there may be alternative ways of displaying this information
that are more suitable_

|Requirement| Function                      | Options                       | Sub-Options                                | Hirarchy  | Notes  |
|---        | ---------------------------   | ---------------------------   | -----------------------------------------  | -------   |----    |
|HL:(4,5,6) | Controller for Current Sensing| Integrated MCU on PCB         | CH340 Driver for USB comms to Raspi        |    3      |        |
|"          |           "                   |           "                   | ublox Wifi module & remote database server |    2      |        |
|"          |           "                   |           "                   | ESP32 performs as MCU controller & Wifi Module|    1   |        |
|"          |           "                   | Arduino Nano 33 IoT| Local Raspberry Pi |    4    | will require 8 port switch to accomidate extra RasPi in ISO Container | 
|"          |           "                   |    "             | Remote Raspberry Pi  |    3    | Will Require external WiFi Antenna to ensure robust Connection|
|           |                               |                               |                                            |           |        |
|HL:(4)     | Power Switching               |N-Channel Low Side MOSFET      |                                            |    3      |        |  
|"          |           "                   |P-Channel High Side MOSFET     |                                            |    1      | Difficult to source suitable MOSFET|  
|"          |           "                   |Relay                          | Normally Closed                            |    2      | Expense       |    
|"          |           "                   |           "                   | Latching                                   |    4      | Expense, Reliability       |      
|           |                               |                               |                                            |           |        |  
|HL:(1,2,3) | Power Supply DC/DC Conversion | COTS Modules                  |                                            |     1     |        |    
|"          |           "                   | ReEngineer & Integrate w/ PCB |                                            |     4     |        |  
|"          |                               |                               |                                            |           |        |
|HL:(7)     | Power Input Connector         | Screw Terminals               |       |  4  |                                             |
|"          |         "                     | XT60 - 60A  Connector         |       |  1  | Quick Disconnect Compared to screw terminals, easy to solder, easier PCB mounting|
|           |                               |                               |                                            |           |        |  
|HL:(7)     | 12v Power Output Connectors   | 2.1mm Barrel Jacks            |       |  4  | Hard to find with range of options, easy to damage|
|"          |           "                   | XT30 - 30A Connector          |       |  1  | Easy to source, many differnt options, well defined specifications|
|           |                               |                               |                                            |           |        |  
|HL:(5)     | Voltage Sensing               | Voltage Divider on 12 bus   | | 1 | Can't be used for 5v bus as MCU would share Vcc ref. Must protect MCU from voltage spikes|
|"          |     "                         | Voltage sense IC            | | 2 | Cant find suitable option, open for reccomendations, could be used for both 12v and 5v bus|
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Circuit Design Optioneering
_Exploration of circuit elements that might be used to meet design objectives_


#### Overvoltage Protection of 5v USB Bus ***[HL.13]***
_The aim of overvoltage protection for the 5v bus, is to protect the raspberry pis in the event +12v power is accidently applied to this bus, pulling all the outputs to 12v
and causing damage. Several possible approaches will be discussed below_

##### Approach 1 - Zener Diode

>    5.1v zener diode reverse biased between the USB outputs 5v rail, and GND.
>    A 100ohm resistor must be between the potential source of over voltage and the zener diode.[^zener]

***Problems with this approach***

>    If the source of the overvoltage is also on the USB bus, this does not nessissarily solve the problem,
>    as the current will flow unimpeded through the zener diode, causing overheating and component failure. 
>    If it fails open circuit (likely) then the protection is no longer active and the 5v bus will be pulled to 12v.

> As the likely source of the overvoltage would be the other USB inputs, each V+ rail would need a 100ohm resistor, with a zener diode on the supply side of the resistor.

##### Approach 2 - Using the MOSFET That is Already There

>    As the USB channel is already designed to be switched off via a logic high signal (+5v) from a microcontroller, 
>    could that same MOSTFET also be triggered with logic high if a fault condition is detected?
>    This solution would have to be passive, i.e not be reliant on the MCU to trigger, as this
>    would delay response and would not stop damage from occuring.
>     <br>
>     A logical OR gate could be used to provide a buffer between the logic HIGH/LOW pin on the microcontroller, and the
>     other system for providing a logical HIGH in the case the 5v bus moves out of tolerance. 1 logical OR gate would be required for each USB output,
>     The output of each logic gate, feeds the gate of each MOSFET

***OverVoltage Detection***

> Voltage Supivisor/detection ICs: <br>
> [TL431 Voltage Reference, Adjustable Shunt](https://uk.farnell.com/on-semiconductor/tl431acdg/voltage-ref-shunt-2-495v-36v-soic/dp/1651199)<br>
>  Voltage is monitored on 5v rail, and IC outputs a logic high if voltage goes outside of set nominal band. <br>
>  
> **Use Case**
> [TI TL431 for Under & Overvoltage Detection]( https://www.ti.com/lit/an/slva987a/slva987a.pdf?ts=1642107195143&ref_url=https%253A%252F%252Fwww.google.com%252F)[^uvov]
> In this case, the V(high) and V(low) thresholds are set via 4 resistors. Calculations outlined below[^tl431] NOTE: Output from LT431 is INVERTED. NOT gate required on
> output for correction.

Resistor Values Estimated:
> R1: 470k      <br>
> R2: 560k      <br>
> Vhigh ~ 5.5v   <br>
> <br>
> R3: 560k   <br>
> R4: 470k   <br>
> Vlow ~ 4.6v


***Implementing This Solution***
> Requires:
> 5 * OR Gates <br>
> 1 * NOT Gate <br>
> <br>
> To make these gates from common gates, it would require: <br>
> 16 * NAND Gates. (3 req for OR gate, 1 for NOT gate) <br>
> OR <br>
> 11 * NOR Gates. (2 req for OR gate, 1 for NOT gate) <br>
> Suggest 
> 3 * [MC74HC02ADG SMD Quad Input NOR Gate](https://uk.farnell.com/on-semiconductor/mc74hc02adg/ic-74hc-cmos-smd-74hc02-soic14/dp/9666893)




#### Power Bus Visual Fault Indications ***[HL.14]***

>   I much prefer the idea of independent indication for each bus, rather than a traffic light with 
>   "All Nominal" "Something Off Nominal" "Everything Off Nominal" indication, as I believe this is,
>   1: less useful than say "A Okay" "B Okay" "C Okay" indications and
>   2: it may also be harder to design if we take a passive approach, i.e: This system should work independently
>   of any microcontrollers.

##### Option 1

>   A window comparator circuit[^comp] could be used to display whether each bus is falling within a predefined "nominal" window,
>   however this circuit would be reliant on stable Vcc, so is not useful for detecting if Vcc is off nominal.
>   An independent linear or LDO regulator could provide independent reference voltages to solve this problem, but 
>   increase in cost & complexity.



##### Option 2

> Led VU Meter, Options detailed [^vu]  [LM3916 Dot/Bar Display Driver](https://cdn.sparkfun.com/datasheets/Components/General%20IC/lm3916.pdf) Monitors 
> analog voltage level and lights up to 10 LEDs sequentially
> Supply Voltage from 3v to 25v - Suggest it runs from 12v rail for each use case. Input can handle from 0 to 35v 

 
> "The LM3916 is extremely easy to apply. A 1.2V fullscale meter requires only one resistor in addition to
> the ten LEDs. One more resistor programs the fullscale anywhere from 1.2V to 12V independent of
> supply voltage. LED brightness is easily controlled
> with a single pot."

> This IC could be set such that a single mid range output lights a green LED, the bottom half of the range lights a yellow LED, and the top half of the range lights a RED led, 
> which would provide under and over voltage indication for each voltage bus.


****************************************************************************************************************************
## [2.2] System Specification Description

_System Specifications for Hardware & Software are derived from and traceable back to the High level Requirements. At this stage optioneering should be
complete, and the design direction finalised._

<!-- NOTE: Origionally I had this document specified as "Low Level Requirements" given past training I had on embedded systems development in aviation, however I 
think this approach was far in excess of what is required for this kind of project, so I have merged "low level requirements" and "system specification" into a single step -->


## [2.3]Example System Specification - [ISOpower]

### [2.3.1]Hardware Specification
_Hardware specification should outline specific hardware devices, circuit design and hardware archetectures chosen to meet high level requirements._

<!-- NOTE: Spreadsheet would be better for comparason of features of components but I see value in documenting major components here too? -->

_Hardware Specification may contain the following subsections:_
- Hardware Architecture
- Major Components - Can be specific or requirements set out for comparason of specific components
- Circuit Design
- Other

### Hardware Architecture & Description

> The hardware will comprise of a single PCB to home the 2 DC/DC converter modules. **[HL.1, HL.2, HL.3]**
> These require local fan cooling for which +12v power and mounting holes will be provided.
>  
> 24v Power input will be via XT60 Connector mounted directly on PCB. **[HL.7]**
> 
> MCU will be integrated to PCB with uBlox Wifi adaptor. MCU will take ADC readings from 2 Allegro ACS712 current sensing modules, one between the DC/DC module and the 12v bus, 
> the other between the 2nd DC/DC module and the 5v bus. **[HL.5]**
> 
> Additionally, a voltage divider will be used with an additional ADC input to monitor the voltage of the 12v bus.**[HL.5, HL.6]** 
> Input to the MCU will be protected by a 5.1v Zener diode, incase of voltage spikes greater than can be mitigated by the voltage divider.
>
> The 5v Bus will be distributed to 5 USB outputs **[HL.9]** via individual high side MOSFET switches for each channel, these will be connected to digital drive pins from the MCU. The Gate pin will be pulled down to GND via pull-down resistor. value to be determined. **[HL.4]**
> Solder bridges will be provided on the PCB to bypass these MOSFETs, in the case they are not required.
> 12v bus power will be distributed to 5 XT30 connectors **[HL.7]** mounted directly on the PCB. **[HL.8]**
>
> Reverse voltage protection will be acheived via a P channel MOSFET[^RevVolt] at +Vcc in **[HL.10]**.
> The system will be protected from overcurrent conditions by a 20A fuse between Vcc in+ and 24v bus. **[HL.11]**.
> 
> PCB dimensions will be 100x120mm. **[HL.12]**

> _Optional:_
> The PCB will contain footprints to allow 12v outputs to be switched via additional MOSFETs, as well as solder bridges to enable the PCB to be used without.

-----------------------------------------------------------------------------------------------------
### Major Components

#### DC/DC Converters
##### Option 1: <br>
**Non-Isolated DC-DC Converter, 3.3 → 15V dc Output, 20A**
| Attribute          	|  Value 	                            |  Notes
|---	                |---	                                |---
| **Part Number:**   	| I6A4W020A033V-001-R               	| 
| **Supplier:**     	| RS Components                      	|   
| **Vin:**          	| 9 - 40v                            	|   	
| **Vout:**           |  3.3 - 24v                          |   
|**Power:**            |    250W                            |     
| **Price:**           |  £44.66                            |   
| **URL:**              | https://uk.rs-online.com/web/p/non-isolated-dc-dc-converters/1813289    |     
| **Notes:**           | -     |    
|**Requirements Met:**  |  (HL.1, HL.2, HL.3)        |




#### P-Channel MOSFET 
_Use: High side power switch_
1. Switching of 5v USB power from bus to individual outputs
2. Switching of 12v power from bus to individual outputs (optional feature)
3. MOSFET is ON when gate is @ 0v, OFF when gate is driven to VDD

_Component Requirements:_
| Attribute | Value        | Notes |
|---        |---           |---    |
| Vds        | > (-)21V      | Drain/Source Breakdown Voltage = Operating Voltage + 70% |
|Id         | > (-)6A        | Max Continuous Drain Current > Stall Current of Motor |
| Vgs       | ~ -4.5       | Gate - Source Threshold Voltage[^Vgs] |
| Rds(on)   | <2 ohm       | Static Drain-to-Source-ON-Resistance[^Rds] @ Vgs |

_In the case the requirements for a component are known, however the specific part is unknown, it would be best to use a [spreadsheet](https://github.com/PanGalacticTech/project_template/blob/main/%5B2A%5Dcomponent_compare.xlsx) to weigh up alternative options._

##### Option 1: <br>
**IRF5305PBF P Channel MOSFET**
|Attribute        | Value                                            |Suitable  | Notes |
|---              |---                                               |---       |---    |
|Part Number:     |  IRF5305PBF                                      |          |       |
|Supplier:        | Farnell                                          | [x]      |       |                 
|Vds              |  -55V                                            | [x]      |       |     
|Id               |  -31A                                            | [x]      |       |  
|Rds(on) @ -10v   |  0.06                                            | [x]      |       |      
|Price:           |  £2.088                                          | [x]      |       |      
|                 |                                                  |          |       |
|URL:             |  [IRF5305PBF - Farnell](https://uk.farnell.com/infineon/irf5305pbf/mosfet-p-55v-31a-to-220/dp/8648255) |         |        |
|Availability     |   In Stock                                       | [x]       |       |
|Notes:           |                                                  |         |       |                                                                            
|Meets Requirements: |                                               | [x]     |       |
    


#### AtMega328P MCU

#### Allego ACS712

#### 
-----------------------------------------------------------------------------------------------------

### Circuit Design & Schematic
_Final Circuit Design, Schematics & Justifications for design_




_____________________________________________________________________________________________________
 
#### [2.3.2]Software Specification

_Specify the software requirements, functions, frameworks and tools required to meet the high level requirements_

#### Tools:
| Tool | Purpose | Justification |
|---   |---      |---            |
|Arduino IDE| Software Development | Speed & ease of development for Arduino IoT functions |
|C++ | Programming Language | Native to Arduino environment |
|InfluxDB | Remote storage of power consumption data | Ease of setup & ease of posting data from remote devices |
|Grafana  | Graphic display of power consumption via web interface | off the shelf solution that can integrate control methods for sending HTTP requests back to controller |


### Software Structure Optioneering

#### Option A

- Arduino running state machine framework
- websocket server responds to HTTP GET requests for control of power channels
- concurrently taking analogRead() of 2 ADC inputs, and sending to external database with HTTP POST requests over wifi
- Data is pulled from database using graphing tool, like Grafana, which is built around a web accessable user interface.

#### Option B

- Arduino running state machine framework
- websocket server responds to HTTP GET requests 
- concurrently taking analogRead() of 2 ADC inputs, and sending to Raspberry Pi over USB communication.
- Raspberry pi requires script to place data received over COM port into web accessable format. 


### Software Specification

- ADC Samples of current sensor will be taken every 250mS  <!-- Let me know if these timings are suitable  -->
- ADC samples of 12v bus voltage will be taken every 250mS <!-- Let me know if these timings are suitable  -->
- Power Channel MOSFETS are "Active Low" Therefore channels will be turned off driven by a HIGH pulse from microcontroller.
- Seperate API for each power channel "on", "off" and "restart",





When to review? 
*System Specification should undertake a review process, to ensure the design meets the clients needs before moving to ***Fabrication*** <br>*


_______________________________________________________________________________________________________________________________________________________

### [2.4] Requirement Matrix

_Info On Requirements Matrix & Link to: [Requirements Matrix Document](https://broken_link.com)_

_______________________________________________________________________________________________________________________________________________________
#### References

- [Rugged Circuits: 10 Ways to Destroy an Arduino](https://www.rugged-circuits.com/10-ways-to-destroy-an-arduino)
- [High Side vs Low Side Switch](https://www.baldengineer.com/low-side-vs-high-side-transistor-switch.html)
- [Important Stuff: MOSFET Specs You Need to Know](https://www.embeddedrelated.com/showarticle/809.php)

*******************************************************************************************************************************************************

#### Notes
**VGS & Rds Explanation**

_Assuming That_

|Attribute | Value | Max Current Flow
|---|---|---|
| Source Voltage | 5v |
|Rds(on) @ Vgs -4.5v | 0.0133 ohm      | 5/0.0133 = 376A |
|Rds(on) @ Vgs -10v   | 0.0083 ohm      | 5/0.0093 = 537A  |


#### Footnotes

[^Rds]: Drain-to-Source On-Resistance <br>
        - If current required is 2.5A, then R=5/2.5: R=2ohm absolute max Rds during operation (in practice must be much lower)   <br>
        - If Vcc = 5v, and gate is pulled to 0v GND, then Vgs ~-5v                                                                <br>
        - Datasheet shows Rds(on) @ Vgs -4.5 will be ~ 0.0133 ohm                                                                     <br>
        - If Vcc = 12v & Gate is pulled to 0v GND, then Vgs ~ -10v therefore Rds(on) ~ 0.0093 ohm                                                       <br>
        - HOWEVER, controller can only provide 5v so will need drivers to fully turn off MOSFET, see[^Vgs(th)]                              <br>

[^Vgs(th)]: Gate-Source Threshold Voltage <br>
            - Datasheet shows that Vgs must be >-2.0V to turn off device  <br>
            - i.e. Id = -250 uA @ Vds (Maximum Drain - Source voltage differential) <br>
            - Therefore if VCC is 12v, Gate Voltage must be driven to >10v to turn off MOSFET <br>

[^V&V]: Verification & Validation - What is it? <br>
        - Verification - _"Does the implementation meet the requirements?"_ <br>
        - Validation   - _"Are the requirements correct"_
        
<!--[^2]: Ruggeduino >5v cutoff circuit -->
      <!-- - ![image](https://user-images.githubusercontent.com/53580358/148758688-282c6b19-230f-4211-98ce-a5ba380fc2d2.png) -->
          
<!-- [^3]: Estimated current consumption of cutoff circuit
      - R = 17kohm, V = 5v. 
      - I=V/R
      - I = (17k / 5) = [2.9\*10^-4] A -->
      
[1v1]: [Arduino: Analog Reference](https://www.arduino.cc/reference/en/language/functions/analog-io/analogreference/)

[^RevVolt]: [Infineon Reverse Voltage Protection Methods](https://www.infineon.com/dgdl/Reverse-Batery-Protection-Rev2.pdf?fileId=db3a304412b407950112b41887722615) <br>
             ![image](https://user-images.githubusercontent.com/53580358/149346402-d48d8c97-2ba4-4139-989e-2c378938aefb.png)

   
[^zener]: [Designing overvoltage protection using zener diodes](https://components101.com/articles/designing-an-overvoltage-protection-circuit-using-zener-diodes)

[^comp]: [op-amp comparator](https://www.electronics-tutorials.ws/opamp/op-amp-comparator.html)

[^vu]: [Single Transistor LED VU meter](https://electronicsarea.com/6-led-vu-meter-using-one-transistor/) <br>
       

[^uvov]: <br> ![image](https://user-images.githubusercontent.com/53580358/149549094-5a31fedb-78fb-4ca3-9df5-5213b09f50f0.png) <br>
![image](https://user-images.githubusercontent.com/53580358/149550148-76459666-dd3e-4661-bf27-dfcf147a4180.png) <br>
[LM431 Datasheet](https://www.ti.com/lit/ds/symlink/lm431.pdf?HQS=dis-dk-null-digikeymode-dsf-pf-null-wwe&ts=1642092638827&ref_url=https%253A%252F%252Fwww.ti.com%252Fgeneral%252Fdocs%252Fsuppproductinfo.tsp%253FdistId%253D10%2526gotoUrl%253Dhttps%253A%252F%252Fwww.ti.com%252Flit%252Fgpn%252Flm431)


[^tl431]: Voltage Window Calculations: <br>
          - Vh=(1+(R2/R)1)*Vref   <br>
          - Vl=(1+(R4/R3))*Vref
