# Title


_This is copied from system_specification incase it is required later, however I think this is best done in a spreadsheet, rather than .md file_

---

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

---

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

_In the case the requirements for a component are known, however the specific part is unknown, it would be best to use a spreadsheet to weigh up altnerative options._




##### Option 1: <br>
**SQP100P06-9m3L Automotive P-Channel 60 V (D-S) 175 °C MOSFET**
|Attribute        | Value                                            |Suitable  | Notes |
|---              |---                                               |---       |---    |
|Part Number:     |  SQP100P06-9m3L                                  |          |       |
|Supplier:        |  unavailable (need to look harder & for alts)    | [ ]      |       |                 
|Vds              |  -60V                                            | [x]      |       |     
|Id               |  -100A                                           | [x]      |       |  
|Rds(on) @ Vgs    |  0.0133                                          | [x]      |       |      
|Price:           |  N/A as Unavailable                              | [ ]      |       |      
|                 |                                                  |          |       |
|URL:             |                                                  |         |        |
|Availability     |   Not Available                                  | [ ]       |       |
|Notes:           |                                                  |         |       |                                                                            
|Meets Requirements: |                                               | [ ]     |       |
    


##### Option 2: <br>
**P-Channel MOSFET, 27 A, 60 V, 3-Pin TO-220AB onsemi FQP27P06**
|Attribute        | Value                                            |Suitable  | Notes |
|---              |---                                               |---       |---    |
|Part Number:     |  FQP27P06                                        |          |       |
|Supplier:        |  RS Online                                  )    | [x]      |       |                 
|Vds              |  -60V                                            | [x]      |       |     
|Id               |  -19.1A  @ 100degC                               | [x]      |       |  
|Rds(on) @ Vgs    |  0.07 @ -10v   <- Unsuitable for 5v Vcc          | [ ]      |       |      
|Price:           |  £1.571 each (50 min order)                      | [x]      |       |      
|                 |                                                  |          |       |
|URL:             |   https://uk.rs-online.com/web/p/mosfets/1784752 |         |        |
|Availability     |  Back order 09/03/22                             | [x]     |       |
|Notes:           |                                                  |         |       |                                                                            
|Meets Requirements: |                                               | [ ]     |       |
    
---

## Old Optioneering Section


### Controller for Current Sensing

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


