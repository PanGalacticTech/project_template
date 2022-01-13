# [2]System Specification - [Project Title]

_This form is intended to assist in optioneering to derive low level hardware & software specification from the ***Validated***  
High Level Requirements Capture Form [[1]requirements_capture.md](https://github.com/PanGalacticTech/project_template/blob/main/%5B1%5Drequirements_capture.md).
Its scope can be adapted to suit projects of varying complexity_ <br>
_______________________________________________________________________________________________________________________________________________________
## [2.1]Validated High Level Requirements

_This section documents the final validated High Level Requirements, these form the basis against which the final system will be compared to determine the 
success of the project_

### Example Project Brief - [ISOpower]
> HL.1. DC/DC converters must be able to provide continuous current draw of 6.7A at 12V ~(80W) for motor power.                   <br>
> HL.2. DC/DC converters must be able to provide peak current draw of 12A at 12V ~(144W)? for motor power in case of stall condition.[^2]  <br>
> HL.3. USB power must be able to provide total of 12.5A @ 5v ~(62.5W).                                 <br>
> HL.4. Each USB channel will have the ability to remotely disable and re-enable power.                                      <br>
> HL.5. Voltage & Current sensing will be available on the 12v Bus   <br>
> HL.6. Voltage & Current sensing will be available on the 5v Bus   <br>
> HL.7. Board must have push-fit or bayonet connectors only. <br>
> HL.8. Power Board will have 5 * 12v outputs from 12v Bus. <br>
> HL.9. Power Board will have 5 * 5v outputs from 5v Bus. <br>
> HL.10. Power board will be protected from reversed supply voltages <br>
> HL.11. Power Board will be protected from over-current conditions<br>

_______________________________________________________________________________________________________________________________________________________
## Design Tradeoffs & Optioneering

_Space to work through different options before deciding on specific low level requirements & system specification for hardware & software_

#### Preamble
_Not good at writing narritives Should something go here?_ <br>

| Product Function              | Options                       | Sub-Options                                | Hirarchy  | Notes  |
|:-----------------------------:|:---------------------------- :|:------------------------------------------ |:---      :|:---    |
| Current Sensing for power bus | Integrated MCU on PCB         | CH340 Driver for USB comms to Raspi        |    2      |        |
|           "                   |           "                   | ublox Wifi module & remote database server |    1      |        |
|           "                   | Arduino Nano 33 IoT           | Local Raspberry Pi                         |    4      |        | 
|           "                   |           "                   | Remote Raspberry Pi                        |    3      |        |
|                               |                               |                                            |           |        |
| Power Switching               |N-Channel Low Side MOSFET      |                                            |    3      |        |  
|           "                   |P-Channel High Side MOSFET     |                                            |    1      |        |  
|           "                   |Relay                          | Normally Closed                            |    2      |        |    
|           "                   |           "                   | Latching                                   |    4      |        |      
|                               |                               |                                            |           |        |  
| Power Supply DC/DC Conversion | COTS Modules                  |                                            |     1     |        |    
|           "                   | ReEngineer & Integrate w/ PCB |                                            |     4     |        |  
|                               |                               |                                            |           |        |

## Voltage & Current Sensing Reporting

**CHC340 interface on Power control Board**
  - Allows MCU to communicate with additional raspberry pi placed in ISO container
    - Advantages 
      - Some
    - Disadvantages
      - If power is lost to box, raspi will not be active. 
      - How is data accessed on raspberry pi? webserver requiring custom development?
 
**Wifi enabled MCU Dev Board - Arduino 33 IoT**
   - MCU Connects to WiFi and can be accessed remotely
     - Advantages
       - No need for additional Raspberry Pi in box
       - HTTP GET requests can be sent by many additional means to switch power on & off.
     - Disadvantages
       - Some  
      
*Either option would allow use of software such as grafana for displaying current & voltage of each power bus, however the 2nd option would make it easier to send HTTP GET
requests from grafana server to MCU in order to actuate power control.*

## MCU Options
**Integrated MCU with PCB**
  - Advantages
    - Less soldering
    - Lower Cost
    - Lower Form Factor - less space
    - More reliable
  - Disadvantages
    - More work needed for PCB development 
    - Need to source additional components - Ublox, Xtal, CH340/USB driver, etc
    
**Complete Dev Board Arduino Nano 33 IoT**
  - Advantages
    - Speed of development
  - Disadvantages
    - Cost
    - Size

_____________________________________________________________________________________________
## Power Control

**Low Side N-Channel MOSFET**
  - Advantages   
    - Lower resistance than p-type = greater current capacity
  - Disadvantages
    - GND reference to switched channels will not be 0v
      
 
**High Side P-Channel MOSFET**
 - Advantages
    - GND Reference to switched channels will be 0v
    - Can boost voltage from DC/DC modules slightly to offset voltage drop across DS       
  - Disadvantages
     - Semiconductor availability due to pandemic!

**Relay**
   - Could use NC Connection or Latching to avoid need for constant power on relay
      - Advantages
        - No Voltage drop issues.
      - Disadvantages
        - Expense
        - Reliability
        - Additional parts required to implement, diodes, drive transistors etc.  

*For now design will progress assuming that High Side P-Channel MOSFET is the best solution for this feature*

_____________________________________________________________________________________________
## Power Supply

**COTS Switch Mode Power Supply Modules**
  - Advantages   
    - Already in use and work
    - Speed of development
  - Disadvantages
    - Cost

**Integrated PSU on PCB**
  - Advantages
    - Cost
  - Disadvantages
    - Everything else
    - In house SMPSU design will not be as efficient or safe as COTS option
    - Not quantified/tested/backed by manufacturers guarentee/years of expertise in PSU design and manufacture


****************************************************************************************************************************
## [2.2]System Specification Description

<- NOTE: Origionally I had this document specified as "Low Level Requirements" given past training I had on embedded systems development in aviation, however I 
think this approach was far in excess of what is required for this kind of project, so I have merged "low level requirements" and "system specification" into a single step

*System Specifications for Hardware & Software are derived from and traceable back to the High level Requirements*


## [2.3]Example System Specification - [ISOpower]

### [2.3.1]Hardware Specification
_Hardware specification should outline specific hardware devices, circuit design and hardware archetectures chosen to meet high level requirements._

<- NOTE: Spreadsheet would be better for comparason of features of components but I see value in documenting major components here too during the optioneering stage?

_Hardware Specification may contain the following subsections:_
- Major Components - Can be specific or requirements set out for optioneering
- System Archetecture
- Circuit Design
- Other

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
| Vds        | < -21V      | Drain/Source Breakdown Voltage = Operating Voltage + 70% |
|Id         | > -6A        | Max Continuous Drain Current > Stall Current of Motor |
| Vgs       | ~ -4.5       | Gate - Source Threshold Voltage[^Vgs] |
| Rds(on)   | <2 ohm       | Static Drain-to-Source-ON-Resistance[^Rds] @ Vgs |

_In the case the requirements for a component are known, however the specific part is unknown, it would be best to use a spreadsheet to weigh up alternative options._



#### Arduino Nano 33 IoT Development Board




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

- ADC Samples of current sensor will be taken every 250mS
- Power Channel MOSFETS are "Active Low" Therefore channels will be turned off driven by a HIGH pulse from microcontroller.
- Seperate API for power channel "on", "off" and "restart"
- 




When to review? 
*System Specification should undertake a review process, to ensure the design meets the clients needs before moving to ***Fabrication*** <br>*


_______________________________________________________________________________________________________________________________________________________

### [2.4]Requirement Matrix

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
        
[^2]: Ruggeduino >5v cutoff circuit
      - ![image](https://user-images.githubusercontent.com/53580358/148758688-282c6b19-230f-4211-98ce-a5ba380fc2d2.png)
          
[^3]: Estimated current consumption of cutoff circuit
      - R = 17kohm, V = 5v. 
      - I=V/R
      - I = (17k / 5) = [2.9\*10^-4] A
   
