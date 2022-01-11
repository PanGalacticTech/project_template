# [2]Low Level Requirements Capture Form - [Project Title]

_This form is intended to assist in optioneering to derive low level hardware & software requirements from the ***Validated***  
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

_______________________________________________________________________________________________________________________________________________________
## Optioneering

_Space to work through different options before deciding on specific low level requirements & system specification_

## Optioneering for Voltage & Current Sensing & Reporting

- CHC340 interface on Power control Board
  - Allows MCU to communicate with additional raspberry pi placed in ISO container
    - Advantages 
      - Some
    - Disadvantages
      - If power is lost to box, raspi will not be active. 
      - How is data accessed on raspberry pi? webserver requiring custom development?
 
 - Wifi enabled MCU Dev Board - Arduino 33 IoT
   - MCU Connects to WiFi and can be accessed remotely
     - Advantages
       - No need for additional Raspberry Pi in box
     - Disadvantages
       - Some  

Either option would allow use of software such as grafana for displaying current & voltage of each power bus, however the 2nd option would make it easier to send HTTP GET
requests from grafana server to MCU in order to actuate power control.
_______________________________________________________________________________________________________________________________________________________
## [2.2]Low Level Requirements Description

*Low Level Requirements are derived from the High level Requirements, with regards to specific hardware & software implementation. They may preceed or accompany design & optioneering*

#### Low Level Requirements should:
- Finalise functionality before exiting design process
- Be traceable to High Level Requirements
- Address internal software interfaces
- Define logic decisions as far as possible
- Be devolved into low level ***Hardware Requirements*** and ***Software Requirements***


#### Requirements should be:
- Specific           - Document a single feature or function implementation at a time.                                                 <br>
- Quantifyable       - Provide quantifyable parameters, boundary limits of operation and/or design tolerences, rather than open ended specifications where possible.                <br>
- Self Contained     - Do not rely on assumptions to deliver information, as different engineers will make different assumptions, which may cause conflicts <-NOTE: Find better wording <br>
- Traceable          - Assign each Low Level requirement an ID number, this will be used to trace any design decisions through the development process. <br>



_Good requirements are the foundation for successful development of a project, as it allows design decisions to be traced through the development process.
it also provides the information required to undertake successful **Verification** & **Validation**[^V&V]._ 


## [2.3]Example Low Level Requirements Capture - [USB Saver Tool]

#### [2.3.1]Hardware Requirements
>
> HW.1. 500mA resettable fuse in series with USB 5v pin, to limit current draw in case of short circuit                      - Trace to: (HL.1, HL.2)   <br>
> HW.2. 5.1v Zener diodes reverse biased between GND and USB Data+, and GND & USB Data, shunts (V > 5.1v) to GND             - Trace to: (HL.3)         <br>
> HW.3. P-Channel MOSFET & voltage sensing circuit on USB Vcc, to disconnect if (V > 5.5v)[^2].                              - Trace to: (HL.3)         <br>
> HW.4. Voltage Divider in voltage sensing circuit current consumption estimated at ~0.3mA[^3].                              - Trace to: (HL.4)         <br>                     
> 


#### [2.3.2]Software Requirements

> SW.1. - *Example ID Number this project does not have any software requirements*

*At the end of this process most High Level requirements should be traceable to at least one low level requirement, and every low level requirement should
be traceable to at least one High Level requirement. At this point, both high level & low level requirements can be placed in a requirement matrix*

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
   
