# [2]Low Level Requirements Capture Form - [Project Title]

_This form is intended to assist in deriving low level hardware & software requirements from the ***Validated***  
High Level Requirements Capture[[1]HL_requirements_capture.md](https://github.com/PanGalacticTech/project_template/blob/main/%5B1%5DHL_requirements_capture.md) Form <br>

_______________________________________________________________________________________________________________________________________________________
## Validated High Level Requirements

_This section simply documents the final validated High Level Requirements_

### Example Project Brief - [USB Saver Tool]

>
> HL.1. Device must protect Client USB from direct short between VCC & GND pins                                            <br>
> HL.2. Device must protect Client USB from current draw >0.5A, as common for older devices.                               <br>
> HL.3. Device must protect Client USB from voltages >5v applied to VCC and Data Pins.                                     <br>
> HL.4. Device should prevent voltages >1v being applied to the client USB GND pin.                                        <br>
> HL.5. Device should operate with <0.2A quiessent current, in order to maximise power available from client USB for embedded project.   <br>
> 

_______________________________________________________________________________________________________________________________________________________
## Low Level Requirements Description

*Low Level Requirements are derived from the High lev Requirements, with regards to specific hardware & software implementation. They may preceed or accompany design *

#### Low Level Requirements should:
- Finalise functionality before exiting design process
- Be traceable to High Level Requirements
- Address internal software interfaces
- Define logic decisions as far as possible


#### Requirements should be:
- Specific           - Document a single feature or function requirement at a time.                                                 <br>
- Quantifyable       - Provide quantifyable parameters, boundary limits of operation and/or design tolerences, rather than open ended specifications where possible.                <br>
- Self Contained     - Do not rely on assumptions to deliver information, as different engineers will make different assumptions, which may cause conflicts <-NOTE: Find better wording <br>
- Traceable          - Assign each Low Level requirement an ID number, this will be used to trace any design decisions through the development process. <br>


--------------------------------------------------------------------------------------
#### Example High Level Requirements
Examples of Good and Poor requirements given below

Good Requirement:
> Device must consume less than 20 mA of current from Client USB

Poor Requirement:
> Device must operate efficiently
 
_It is better to be specific early then revise this requirement as the design progresses, than leave options totally open_ <- NOTE: This is my personal experience, but alternative paradigms are well worth discussion<br>

--------------------------------------------------------------------------------------

More Advice on Requirements:
- Preface requirements with qualifiers "Must" or "Should" to indicate whether requirements are hard or soft requirements <- NOTE: Tired friday night wording, this can be improved.<br>
- Provide reasoning behind requirements if otherwise unclear, this provides a record of why early design decisions were made.<br>
- Reference industrial standards where relevent. This will help define derived requirements during the next step of the development timeline. <br>

_Good requirements are the foundation for successful development of a project, as it allows design decisions to be traced through the development process.
it also provides the information required to undertake successful **Verification** & **Validation**[^V&V]._ 


### Example Requirements Capture

>
> HL.1. Device must protect Client USB from direct short between VCC & GND pins                                            <br>
> HL.2. Device must protect Client USB from current draw >0.5A, as common for older devices.                               <br>
> HL.3. Device must protect Client USB from voltages >5v applied to VCC and Data Pins.                                     <br>
> HL.4. Device should prevent voltages >1v being applied to the client USB GND pin.                                        <br>
> HL.5. Device should operate with <0.2A quiessent current, in order to maximise power available from client USB for embedded project.   <br>
> 

When to review? 
*High level requirements should undertake a review process, ***Validation***, to ensure they meet the clients needs, before any further development takes place. <br>
*



_______________________________________________________________________________________________________________________________________________________
## Design Optimisation?

_This section should probably go later, not sure. Should it be included in requirements?
What parameters of the design should be minimised/maximised?_


[^V&V]: Verification & Validation - What is it? <br>
        - Verification - _"Does the implementation meet the requirements?"_ <br>
        - Validation   - _"Are the requirements correct"_