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

#### Arduino Nano 33 IoT Development Board
