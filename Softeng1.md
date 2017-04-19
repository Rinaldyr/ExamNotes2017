# Software Engineering 1 Revision Notes


----

# Software Development Process
### Requirements Engineering
Deals with establishting the needs of stakeholders that are to be solved by the software. Cost of requirements error correction increases as the development process progresses
* Elicitation - Collection of collection of requirements from stake holders
* Analysis - Study and deeper understanding of collected requirements
* Specification - Collected requirements are suitably represented, organised, and shared in order to be presented
* Validation - Ensure requirements are complete, consistent, not made redundant etc. Ensure all requirements satisfy a set of important properties
* Management - Changes to requirements during the lifetime of the software
Iterative process in which different phases are covered in an iterative fashion.
### Software Design
Consists of Design activities:

    Architectual Design -> Abstract Specification -> Interface Design -> Component Design -> Data Strucure -> Algorithm Design

Go from a high level to a low level view. Each design activity corresponds to a design product which describe characteristics of the system.

    System Structure -> Software Specification -> Interface Specification -> Component Specification -> Data Structure Specification -> Algorithm Specification

### Implementation
Four principles that can affect the way software is implemented:
* Reduction of complexity - Aims to build software that is simple to understand and use.
* Anticipation of diversity - Takes into account that software construction may change in various ways over time. Software evolves, and in many cases, changes are unexpected and therefore we must be able to anticipate some of these changes.
* Structuring for validation - Design for testability. We want to build software that is easily testable in the subsequent validation and vertification activities.
* Use of internal and external standards - Ensures that software conforms to any standards put in place. Could be coding conventions or standards set within the domain of the software.

### Verification and Validation
Validation: Did we build the right system? - Did we build the system that is needed by the stakeholder?

Vertification: Did we build the system right? - Given the specfication, did we build the correct system described?

Verification can be performed at:
* Unit level - Testing functionality of individual units work as expected.
* Integration level - Testing interaction between different units. Ensuring different modules talk to each other in the right way.
* System testing - Testing system as a whole. Ensuring all of the system work together in the right way. (Also testing for stress testing, robustness testing, etc.)
### Maintenance
The activity that maintains the software throughout its lifecycle. Once software is released, changes may need to be made. Maintance come from 3 types:
* Corrective maintenace: Changes made from submitted bug reports to fix potential bugs.
* Perfective maintenance: To acccomodate new feature requests. Also to improve the software (efficiency, ease-of-use, etc.)
* Adaptive maintenance: To take care of environment changes.

Once the maintenance has been perfomed, the developer(s) will release the new version and the cycle continues throughout the lifetime of the software.

Every time maintenance is performed, regression testing must be performed - testing existing functionality after the software has been modified to ensure new bugs have not been introduced. 

----

# Software Lifecycle Model
Determine the order of the different activities. Determine the transition criteria of the activies - how/when to go from one phase to a sebsequent one.
## Models
### Waterfall Model
![](http://istqbexamcertification.com/wp-content/uploads/2012/01/Waterfall-model.jpg)
At the end of each phase, there is a review to determine if the project is ready to advance to the next phase.

Performs well for projects that have a well defined specification - stable product definition, technologies involved are well understood and the domain is well known. 

(+) An advantage is it allows developers to find errors early. 

(-) However, it is not flexible. Normally it is difficult to fully specifiy requirements at the beginning of the project. Far from ideal when dealing with projects where requirements change, developers are not experts in the domains, or technologies used are new/evolving. Not ideal for most real world projects.

### Spiral Model
![](https://upload.wikimedia.org/wikipedia/commons/thumb/e/ec/Spiral_model_%28Boehm%2C_1988%29.svg/1200px-Spiral_model_%28Boehm%2C_1988%29.svg.png)
Incremental, risk-oriented lifecycle model that has 4 main phases:
1. Determine objects - Requirements gathered
2. Identify and resolve risks: Risks and alternate solutions identified and prototype is produced
3. Development and testing
4. Plan the next iteration - Output is evaluated and the next iteration is planned

Each phase is visited in an iterative way, where more and more is learnt about the software, risks and final solution/release.

(+) Extensive risk analysis reduces changes of the project failing
(+) Functionality can be added at a later phase due to its iterative nature
(+) Software is produced early in the lifecycle. At each iteration, there is something to show. 

(-) Risk analysis requires a highly specific expertise
(-) Highly dependent on risk analysis.
(-) More complex than other models (e.g. Waterfall) which makes it costly to implement.

### Evolutionary Prototyping
![](http://crackmba.com/wp-content/uploads/2012/02/Evalutionalry-prototype.jpg)
The system is continously refined and rebuilt so it is an ideal process when not all requirements are well understood which is a common situation. Developers start by developing the parts of the sytem that they understand instead of developing all features (including features that may not be very clear). 

Prototype is fed to the customer and feedback is used for the next iteration in which changes are made to current features or new features are added.

(+) Developers get immediate feedback as soon as they release the prototype and show it to the customer. Risk of implementing the wrong system is minimised.

(-) Difficult to plan in advance how long the development will take. Don't know how much time is needed for each iteration. 

### Rational Unified Process (RUP)
![](https://upload.wikimedia.org/wikipedia/commons/1/19/Development-iterative.png)
* Inception - Determine the scope and domain of the project to perform initial cost and budget estimates.
* Elaboration - Focus on domain analysis and basic architecture
* Construction - Bulk of development and implementation/testing
* Transisition - System goes from development to production so that it becomes avaiable to users.

### Agile - Test Driven Development
![](http://luizricardo.org/wordpress/wp-content/upload-files/2014/05/tdd_flow.gif)  

### Choosing a model
* Determine how much of the requirements are understood?
* Will requirements change? 
* Expected lifetime of the project?
* Level of risk involved? Do we know the domain and techonologies?
* Schedule constraints?
* Expected interaction with the management and customer?
* Expertise of people involved?

----

# Requirements Engineering
Establishing the services that the stakeholders require from the software system. Also establishes the constraints on which the system operates and is developed.

Specification is a bridge between the Application Domain and the Machine Domain.

Many errors are made in requirements specification. Detecting errors early can dramatically decrease software costs.

Focuses on what and not how.

Identifying purpose = defining requirements

Potential difficulties:
* Complexity of the requirements
* People don't know what they want
* Changing requirements
* Multiple stakeholders = likely to have conflicting wants/needs

Completeness: Ensuring all needed functionality have been identified

Pertinence: Ensuring only the needed functionality is identified and not functionality that will not be used.

 





