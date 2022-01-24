# [1]   Requirements Capture Form - [Project Title]

_This form is intended to assist in capturing the requirements for new Open Source Embedded Systems Projects, <br>
its scope & implementation can be adapted to suit projects of varying complexity._

***************************************************************************************************************************************************************
## [1.1]Project Brief
_The project brief should contain an outline of the proposed project, with enough detail to derive a comprehensive list of requirements. The format, layout and
information provided can be tailored to project complexity_


### Example Project Brief - [ISOpower]

> PCB required to host DC-DC converters, to convert 24V DC power supply into operating voltages for ISO Experimental containers. 
> The containers house 4 experiments, each requires 12v for motor power, and 5v for raspberry pi via USB.
> Each USB 5v channel requires a power switch that can be actuated remotely.
> 
> Total current draw for 12v & 5v bus should be monitored via a microcontroller, with a interface allowing control
> over power switches on the board.
> Additional 5v & 12v power channel is required for future proofing, additional control infrastructure etc.
> 
> LED indication of correct voltage for 24v, 12v & 5v bus.
> Cooling should be provided for DC/DC Converters.
> Estimated power requirements set out in sub-system specification below.


#### Optional Features
_Any features or functions that are optional should be stated here_  <br>
>  1. Report Status & current usage for sustainabilty and maintainance purposes.
>  2. Individual remote control of channel power.
>  3. Use only connection types that can be operated with one hand. i.e. avoid screw terminals.


#### Sub-Systems Specifications
_Specific requirements for subsystems should be documented at this stage_    <br>
> a. steady power usage of 20W 1.7A at 12V (Maxxon AMax 32 236668 Graphite brushes, 20 Watt)[Estimated value, may change in future iterations] <br>
> b. peak power usage of 69W 5.7A at 12V (when motor stalled)  [Estimated value, may change in future iterations] <br>
> c. 5V power for the Raspberry pis, typically 2.5A per Raspberry Pi; four raspberry pi connections, 1 connection for power to microcontroller monitoring chip. <br>

#### General Specifications/Requirements
_These specifications are going to be valid for most projects developed using this framework_
> 4. Circuit should be protected from reverse supply voltages. <br>
> 5. Circuit should be fuse protected <br>
> 6. 5v bus should be protected from over voltage conditions.

#### Design Optimisation
_What parameters of the design should be minimised/maximised?_

#### Design Tradeoffs
_Space for discussion and weighing up of features that may or may not be required_ <br>

#### Quality Assurance 
_What quality assurances or requirements, if any, need to be established?_

#### Notes
_Any Extra Notes?_
Software Reqirements: 
JSON commands/interaction with microcontroller -> reporting back



### Existing Products & COTS (Consumer Off The Shelf)
_The project brief may also contain information on currently available similar products and a brief description of the features they lack and a 
quick review of whether they can fullfill the aims of the project._


_______________________________________________________________________________________________________________________________________________________
## [1.2]High Level Requirements Description

*This section should contain a numbered list of all the specific high level requirements that can be gathered from the project brief, or from the client directly. <br>
High Level Requirements capture the intent and function of a system, without going into specifics of how these will be achieved or any specific implementations. I.E. They take a Black Box approach*

#### High Level Requirements should address:
> - Client Needs
> - Features & Functionality
> - Interfaces
> - Performance
> - Quality Attributes
> - Design Constraints
> - Regulations
> - Safety & Security
> - System

#### Requirements should be:
> - Specific           - Document a single feature or function requirement at a time.                                                 <br>
> - Quantifyable       - Provide quantifyable parameters, boundary limits of operation and/or design tolerences, rather than open ended specifications where possible.<br>
> - Self Contained     - Do not rely on assumptions to deliver information, as different engineers will make different assumptions, which may cause conflicts 
> - Traceable          - Assign each requirement an ID number, this will be used to trace any design decisions through the development process. <br>

_Good requirements are the foundation for successful development of a project, as it allows design decisions to be traced through the development process.
it also provides the information required to undertake successful **Verification** & **Validation**[^V&V]._ 


_______________________________________________________________________________________________________________________________________________________
## [1.3]Example Requirements Capture - [ISOpower]


> HL.1. DC/DC converters must be able to provide continuous current draw of 6.7A at 12V ~(80W) for motor power, from supplied 24v input.                   <br>
> HL.2. DC/DC converters must be able to provide peak current draw of 12A at 12V ~(144W) for motor power, from supplied 24v input, in case of stall condition.[^2]    <br>
> HL.3. USB power must be able to provide total of 12.5A @ 5v ~(62.5W), from supplied 24v input.                                  <br>
> HL.4. Each USB channel will have the ability to remotely disable and re-enable power.                                      <br>
> HL.5. Voltage & Current monitoring of 12v bus, accessable remotely.   <br>   <!-- This is a weak requirement, how often should data be sampled is a nessissary component of this requirement -->
> HL.6. Current monitoring of 5v bus, accessable remotely  <br>    <!-- This is a weak requirement, how often should data be sampled is a nessissary component of this requirement -->
> HL.7. Board must have push-fit or bayonet connectors for motors & offboard hardware. <br>
> HL.8. Power Board will have 5 * 12v outputs from 12v Bus. <br>
> HL.9. Power Board will have 5 * 5v outputs from 5v Bus. <br>
> HL.10. Power board will be protected from reversed supply voltages <br>
> HL.11. Power Board will be protected from over-current conditions<br>
> HL.12. PCB must have same footprint & mounting screwholes as V1, to aid in assembly <br>
> HL.13. 5v bus will be protected from over-voltage condition. <br>
> HL.14. Each power bus will display visual (LED) indication of correct operation or fault conditions (over/under voltage) <br>


<!-- NOTE: 
High level
Include a microcontroller which communicates over USB, with a JSON-based interface allowing control over any power switches on board, and can report any measurements made.
It is my understanding this is not a high level requirement, as it is a specific low level hardware & software requirement. I have modified some of the other requirements
 to make it clear that the data & power channel actuation should be accessable remotly, but have not gone into specifics of how that would be achieved-->



When to review? <br>
*High level requirements should undertake a review process,* ***Validation***, *to ensure they meet the clients needs, before any further development takes place.*<br> 

<!--After review, please see the next document in the design process* [[2]Low Level Requirements Capture](https://github.com/PanGalacticTech/project_template/blob/main/%5B2%5DLL_requirements_capture.md) -->


_______________________________________________________________________________________________________________________________________________________



[^V&V]: Verification & Validation - What is it? <br>
        - Verification - _"Does the implementation meet the requirements?"_ <br>
        - Validation   - _"Are the requirements correct"_
        
[^2]: Assumption made that only 1 motor will be in stall condition, with others running typically.
