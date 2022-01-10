# Requirements Capture Form - [Project Title]

_______________________________________________________________________________________________________________________________________________________
## Project Brief
_The project brief should contain an outline of the proposed project, with enough detail to derive a comprehensive list of requirements._

### Example Project Brief
## USB Saver Tool

>
> The client requires a PCB to protect laptops and personal devices from damage
> while programming or powering USB enabled embedded systems projects during development & prototyping
>

_______________________________________________________________________________________________________________________________________________________
## Requirements Capture

*This section should contain a numbered list of all the specific requirements that can be gathered from the project brief, or from the client directly.*

Requirements should be:
- Simple & Specific - document a single feature or function at a time                                           <br>
- Precise & Quantifyable - Provide limits or quantifyable parameters, rather than open ended specifications      <br>
    (e.g. "Device must consume less than 20 mA of current from Client USB", rather than "Device must operate efficiently")      <br>
    - It is better to be specific early then revise this requirement as the design progresses, than leave options totally open <- NOTE: This is my personal experience, but <br> alternative paradigms are well worth discussion<br>
- Preface requirements with qualifiers "Must" or "Should" to indicate whether requirements are hard or soft requirements <- Tired friday night wording, this can be improved.<br>
- Provide reasoning behind requirements if otherwise unclear, this provides a record of why early design decisions were made.<br>


### Example Requirements Capture

>
> 1. Device must protect Client USB from direct short between VCC & GND pins                                          <br>
> 2. Device must protect Client USB from current draw >0.5A, as common for older devices.                             <br>
> 3. Device must protect Client USB from voltages >5v applied to VCC and Data Pins.                                   <br>
> 4. Device should prevent voltages >1v being applied to the client USB GND pin.                                      <br>
> 5. Device should operate with <0.2A quiessent current, in order to maximise current available for embedded project. <br>
> 6. 


_______________________________________________________________________________________________________________________________________________________
## Design Optimisation?

_This section should probably go later, not sure. Should it be included in requirements?
What parameters of the design should be minimised/maximised?
