# [2]Low Level Requirements Capture Form - [Project Title]

_This form is intended to assist in deriving low level hardware & software requirements from the ***Validated***  
High Level Requirements Capture Form [[1]HL_requirements_capture.md](https://github.com/PanGalacticTech/project_template/blob/main/%5B1%5DHL_requirements_capture.md).
Its scope can be adapted to suit projects of varying complexity_ <br>
_______________________________________________________________________________________________________________________________________________________
## [2.1]Validated High Level Requirements

_This section documents the final validated High Level Requirements_

### Example Project Brief - [USB Saver Tool]

>
> HL.1. Device must protect Client USB from direct short between VCC & GND pins                                            <br>
> HL.2. Device must protect Client USB from current draw >0.5A, as common for older devices.                               <br>
> HL.3. Device must protect Client USB from voltages >5v applied to VCC and Data Pins.                                     <br>
> HL.4. Device should prevent voltages >1v being applied to the client USB GND pin.                                        <br>
> HL.5. Device should operate with <0.2A quiessent current, in order to maximise power available from client USB for embedded project.   <br>
> 

_______________________________________________________________________________________________________________________________________________________
## [2.2]Low Level Requirements Description

*Low Level Requirements are derived from the High lev Requirements, with regards to specific hardware & software implementation. They may preceed or accompany design*

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


--------------------------------------------------------------------------------------
#### Example Low Level Requirements
Examples of Good and Poor requirements given below <- NOTE: Need better examples or are they even eissary at this point?

Good Requirement:
> Device must consume less than 20 mA of current from Client USB

weak Requirement:
> Device must operate efficiently
 
_It is better to be specific early then revise this requirement as the design progresses, than leave options totally open_ <- NOTE: This is my personal experience, but alternative paradigms are well worth discussion<br>

--------------------------------------------------------------------------------------

More Advice on Requirements:
- Provide reasoning behind requirements if otherwise unclear, this provides a record of why design decisions were made.<br>
- Reference industrial standards where relevent.<br>

_Good requirements are the foundation for successful development of a project, as it allows design decisions to be traced through the development process.
it also provides the information required to undertake successful **Verification** & **Validation**[^V&V]._ 


## [2.3]Example Low Level Requirements Capture - [USB Saver Tool]

#### Hardware Requirements
>
> HW.1. 500mA resettable fuse in series with USB 5v pin, to limit current draw in case of short circuit[^2] - Trace to: (HL.1, HL.2)   <br>
> HW.2. 5.1v Zener diodes reverse biased between GND and VCC, GND and USB Data+, and GND& USB Data-     - Trace to: (HL.3)         <br>
> 
> HL.1. Device must protect Client USB from direct short between VCC & GND pins                                            <br>
> HL.2. Device must protect Client USB from current draw >0.5A, as common for older devices.                               <br>
> HL.3. Device must protect Client USB from voltages >5v applied to VCC and Data Pins.                                     <br>
> HL.4. Device should prevent voltages >1v being applied to the client USB GND pin.                                        <br>
> HL.5. Device should operate with <0.2A quiessent current, in order to maximise power available from client USB for embedded project.   <br>

#### Software Requirements

> SW.1. - *Example ID Number this project does not have any software requirements*

*At the end of this process every High Level requirement should be traceable to at least one low level requirement, and every low level requirement should
be traceable to at least one High Level requirement. At this point, both high level & low level requirements can be placed in a requirement matrix*

When to review? 
*Low level requirements should undertake a review process, ***Validation***, to ensure they meet the clients needs and, before any further development takes place. <br>
*

_______________________________________________________________________________________________________________________________________________________
## Design Optimisation?

_What parameters of the design should be minimised/maximised?_

_______________________________________________________________________________________________________________________________________________________

_______________________________________________________________________________________________________________________________________________________

### [2.4]Requirement Matrix



#### References

- [Rugged Circuits: 10 Ways to Destroy an Arduino](https://www.rugged-circuits.com/10-ways-to-destroy-an-arduino)

#### Footnotes



[^V&V]: Verification & Validation - What is it? <br>
        - Verification - _"Does the implementation meet the requirements?"_ <br>
        - Validation   - _"Are the requirements correct"_
        
[^2]: Ruggeduino >5v cutoff circuit
          - ![image](https://user-images.githubusercontent.com/53580358/148758688-282c6b19-230f-4211-98ce-a5ba380fc2d2.png)
