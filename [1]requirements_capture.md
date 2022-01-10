# [1]Requirements Capture Form - [Project Title]

_______________________________________________________________________________________________________________________________________________________
## Project Brief Description
_The project brief should contain an outline of the proposed project, with enough detail to derive a comprehensive list of requirements._

### Example Project Brief - [USB Saver Tool]

> The client requires a PCB to protect laptops and personal devices from damage
> while programming or powering USB enabled embedded systems projects during
> development & prototyping.


_The project brief may also contain information on currently available similar products, and a brief description of the features they lack_ <- NOTE: Would this be too much info for this kind of doc? Should a seperate process be carried out on receipt of the requirements to look for existing COTS (Consumer Off The Shelf) options?

_______________________________________________________________________________________________________________________________________________________
## Requirements Capture Description

*This section should contain a numbered list of all the specific requirements that can be gathered from the project brief, or from the client directly.*

#### Requirements should be:
- Specific           - Document a single feature or function requirement at a time.                                                 <br>
- Quantifyable       - Provide quantifyable parameters or boundary limits of operation, rather than open ended specifications where possible.                <br>
- Self Contained     - Do not rely on assumptions to deliver information, as different engineers may make different assumptions, which may conflict <- Find better wording <br>

--------------------------------------------------------------------------------------
#### Example Requirements
Examples of Good and Poor requirements given below

Good Requirement:
> Device must consume less than 20 mA of current from Client USB

Poor Requirement
> Device must operate efficiently

_It is better to be specific early then revise this requirement as the design progresses, than leave options totally open_ <- NOTE: This is my personal experience, but alternative paradigms are well worth discussion<br>
--------------------------------------------------------------------------------------

More Advice on Requirements:
- Preface requirements with qualifiers "Must" or "Should" to indicate whether requirements are hard or soft requirements <- Tired friday night wording, this can be improved.<br>
- Provide reasoning behind requirements if otherwise unclear, this provides a record of why early design decisions were made.<br>
- Reference industrial standards where relevent. This will help define derived requirements during the next step of the development timeline. <br>

_Good requirements are the foundation for successful development of a project, as it allows design decisions to be traced through the development process.
it also provides the information required to undertake successful **Verification** & **Validation**[^V&V]. 


### Example Requirements Capture

>
> 1. Device must protect Client USB from direct short between VCC & GND pins                                            <br>
> 2. Device must protect Client USB from current draw >0.5A, as common for older devices.                               <br>
> 3. Device must protect Client USB from voltages >5v applied to VCC and Data Pins.                                     <br>
> 4. Device should prevent voltages >1v being applied to the client USB GND pin.                                        <br>
> 5. Device should operate with <0.2A quiessent current, in order to maximise current available for embedded project.   <br>
> 


_______________________________________________________________________________________________________________________________________________________
## Design Optimisation?

_This section should probably go later, not sure. Should it be included in requirements?
What parameters of the design should be minimised/maximised?_


[^V&V]: Verification & Validation - What is it? <br>
        - Verification - _"Does the implementation meet the requirements?"_ <br>
        - Validation   - _"Are the requirements correct"_
