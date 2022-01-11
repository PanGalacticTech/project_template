_______________________________________________________________________________________________________________________________________________________
## [1.2]High Level Requirements Capture Description

*This section should contain a numbered list of all the specific high level requirements that can be gathered from the project brief, or from the client directly. <br>
High Level Requirements capture the intent and function of a system, without going into specifics of how these will be achieved or any specific implementations. I.E. They take a Black Box approach*

#### High Level Requirements should address:
- Client Needs
- Features & Functionality
- Interfaces
- Performance
- Quality Attributes
- Design Constraints
- Regulations
- Safety & Security
- System

#### Requirements should be:
- Specific           - Document a single feature or function requirement at a time.                                                 <br>
- Quantifyable       - Provide quantifyable parameters, boundary limits of operation and/or design tolerences, rather than open ended specifications where possible.                <br>
- Self Contained     - Do not rely on assumptions to deliver information, as different engineers will make different assumptions, which may cause conflicts <-NOTE: Find better wording <br>
- Traceable          - Assign each requirement an ID number, this will be used to trace any design decisions through the development process. <br>

--------------------------------------------------------------------------------------
#### Example High Level Requirements
Examples of Good and Weak requirements given below

Good Requirement:
> Device must consume less than 20 mA of current from Client USB

Weak Requirement:
> Device must operate efficiently
 
_It is better to be specific early then revise this requirement as the design progresses, than leave options totally open_ <- NOTE: This is my personal experience, but alternative paradigms are well worth discussion<br>

--------------------------------------------------------------------------------------







More Advice on Requirements:
- Preface requirements with qualifiers "Must" or "Should" to indicate whether requirements are hard or soft requirements <- NOTE: Tired friday night wording, this can be improved.<br>
- Provide reasoning behind requirements if otherwise unclear, this provides a record of why early design decisions were made.<br>
- Reference industrial standards where relevent. This will help define derived requirements during the next step of the development timeline. <br>

_Good requirements are the foundation for successful development of a project, as it allows design decisions to be traced through the development process.
it also provides the information required to undertake successful **Verification** & **Validation**[^V&V]._ 


[^V&V]: Verification & Validation - What is it? <br>
        - Verification - _"Does the implementation meet the requirements?"_ <br>
        - Validation   - _"Are the requirements correct"_
