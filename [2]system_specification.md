# [2]System Specification - [Project Title]

_This form is intended to assist in optioneering to derive low level hardware & software specification from the ***Validated***  
High Level Requirements Capture Form [[1]HL_requirements_capture.md](https://github.com/PanGalacticTech/project_template/blob/main/%5B1%5DHL_requirements_capture.md).
Its scope can be adapted to suit projects of varying complexity_ <br>
_______________________________________________________________________________________________________________________________________________________
## [2.1]Validated High Level Requirements

_This section documents the final validated High Level Requirements_

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
> HL.11. Power Board will be protected from over-current conditions by a fuse <br>

_______________________________________________________________________________________________________________________________________________________
## Optioneering

_Space to work through different options before deciding on specific low level requirements & system specification_

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
     - ?

**Relay**
   - Could use NC Connection or Latching to avoid need for constant power on relay
      - Advantages
        - No Voltage drop issues.
      - Disadvantages
        - Expense
        - Reliability
        - Additional parts required to implement, diodes, drive transistors etc.  

*For now design will progress assuming that High Side P-Channel MOSFET is the best solution for this feature*

_______________________________________________________________________________________________________________________________________________________
## [2.2]System Specification Description

<- NOTE: Origionally I had this document specified as "Low Level Requirements" given past training I had on embedded systems development in aviation, however I 
think this approach was far in excess of what is required for this kind of project, so I have merged "low level requirements" and "system specification" into a single step

*System Specifications for Hardware & Software are derived from and traceable back to the High level Requirements*


## [2.3]Example System Specification - [ISOpower]

### [2.3.1]Hardware Specification
_Hardware specification should outline specific hardware devices, circuit design and hardware archetectures chosen to meet high level requirements._

<- NOTE: Spreadsheet would be better for comparason of features of components but I see value in documenting major components here too?

_Hardware Specification may contain the following subsections:_
- Major Components - Can be specific or requirements set out for optioneering
- System Archetecture
- Circuit Design
- Other

### Major Components

#### DC/DC Converters
##### Option 1:
|**Non-Isolated DC-DC Converter, 3.3 → 15V dc Output, 20A**|
| Attribute          	|  Value 	|  
|---	                |---	    |
| **Part Number:**   	| I6A4W020A033V-001-R      	| 
| **Supplier:**     	| RS Components    	|   
| **Vin:**          	| 9 - 40v    	|   	
| **Vout:**           |  3.3 - 24v     |   
|**Power:**            |    250W       |     
| **Price:**           |  £44.66       |   
| **URL:**              | https://uk.rs-online.com/web/p/non-isolated-dc-dc-converters/1813289    |     
| **Notes:**           | -     |    
|**Requirements Met:**  |  (HL.1, HL.2, HL.3)        |



#### P-Channel MOSFET 
_Use:_
- Switching of 5v USB power from bus to individual outputs
- Switching of 12v power from bus to individual outputs (optional feature)
_Component Requirements:_
| Attribute | Value | Notes |
|---        |---    |---    |
| Vds | >21V | Drain/Source Breakdown Voltage = Operating Voltage + 70% |
|Id         | 6A    | Max Continuous Drain Current > Stall Current of Motor |
| Vgs       |       | Gate - Source Threshold Voltage[^Vgs] |
| Rds(on)   |       | Static Drain-to-Source-ON-Resistance |


##### Option 1:               SQP100P06-9m3L Automotive P-Channel 60 V (D-S) 175 °C MOSFET
|Attribute | Value |Meets Requirement| Notes |
|---|---|---|---|
|**Part Number:**            |  SQP100P06-9m3L                                  |       |         
|**Supplier:**               |  unavailable (need to look harder & for alts)    |        |                               
|Vds                         |  -60V                                             | [x]   |                  
|Id                          |                                                  |        |              
|**Power:**                  |                                                  |        |                 
|**Price:**                  |                                                  |        |                
|                            |                                                  |         |
|**URL:** |https://uk.rs-online.com/web/p/non-isolated-dc-dc-converters/1813289 |        |   
|     |    |    |
|**Notes:**| |  |                                                                            
|**Meets Requirements:**     |    (HL.1, HL.2, HL.3)                              |      |         






_____________________________________________________________________________________________________

Incomplete/ Leftover from Low Level Requirements Doc

 
[ ] i6A4W Series DC/DC Step down converter selected for 12v bus power
 
 - Trace to: (HL.1, HL.2)   <br>
 HW.2. 5.1v Zener diodes reverse biased between GND and USB Data+, and GND & USB Data, shunts (V > 5.1v) to GND             - Trace to: (HL.3)         <br>
 HW.3. P-Channel MOSFET & voltage sensing circuit on USB Vcc, to disconnect if (V > 5.5v)[^2].                              - Trace to: (HL.3)         <br>
 HW.4. Voltage Divider in voltage sensing circuit current consumption estimated at ~0.3mA[^3].                              - Trace to: (HL.4)         <br>                     


#### [2.3.2]Software Requirements

> SW.1. - *Example ID Number this project does not have any software requirements*


When to review? 
*Low level requirements should undertake a review process,* ***Validation***, *to ensure they meet the clients needs and, before any further development takes place. <br>*

_______________________________________________________________________________________________________________________________________________________
## Design Optimisation?

_What parameters of the design should be minimised/maximised?_

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

- Assuming that
        |Attribute | Value | Current Flow
        |---|---|---|
        | Source Voltage | 5v |
        |Rds(on) @ Vgs -4.5v | 0.0133 ohm      | 5/0.0133 = 376A ??? This doesn't make sense to me |
        Rds(on) @ Vgs -10v   | 0.0083 ohm      | 5/0.0093 = 537A ??? |


#### Footnotes



[^V&V]: Verification & Validation - What is it? <br>
        - Verification - _"Does the implementation meet the requirements?"_ <br>
        - Validation   - _"Are the requirements correct"_
        
[^2]: Ruggeduino >5v cutoff circuit
      - ![image](https://user-images.githubusercontent.com/53580358/148758688-282c6b19-230f-4211-98ce-a5ba380fc2d2.png)
          
[^3]: Estimated current consumption of cutoff circuit
      - R = 17kohm, V = 5v. 
      - I=V/R
      - I = (17k / 5) = [2.9\*10^-4] A
   
