## Requirements in Plan-Driven Software Development
### Plan-And-Document Software Methodologies - Revisit
The stages in most software methodologies consist of these phases:
- Requirements, Analysis, Design, Code & Integrate, Test/QA, Deploy/Operate/Maintain
The goal for planning and documenting is to make software engineering as predictable in budgeting and scheduling as possible
- Requirements elicitation
- Requirements documentation
- Cost estimation
- Sceduling and monitoring schedule
- Change management for requirements, cost and schedule
- Ensuring implementation matches requirements features
- Risk analysis and management

In very traditional methodologies, requirement document is usually very detailed and extensive
- Often done in a Word document with a very detailed template
- Requirements are named and numbered to support traceability
![[Pasted image 20231003110946.png]]
### Requirement Elicitation - Techniques
- Interviewing stakeholders
	- Information discussions or formal questions
		- Gathers insights about requirements and possible solutions
- Cooperatively create scenarios
	- Initial state, happy and sad paths, concurrent and final states
- Create Use Cases
	- User and system interactions to realize functions

### Requirements Elicitation
- Functional
	- Details matter: What information goes in and out
	- Interactions between features
	- How should the system behave when under erroneous conditions
- Non-functional
	- Include performance, security, and usability
- Note that different stakeholders often have different thoughts on this

### Requirements Documentation
- Software Requirements Specifications (SRS) process
	- 100s of pages, IEEE 930-1998 standard recommended practice for SRS
- Stakeholders read SRS document, build a basic prototype or generate test cases to check:
	- Validity
	- Consistency
	- Completeness
	- Feasibility
- Estimate budget and schedule based on SRS

### Why Software Projects Fail?
- Software projects often fail because they often exceed their allocated budget due to poor estimation and / or unforeseen challenges
- Another reason why software projects fail is because the software becomes difficult to update due to poor design, no documentation, or technological obsolescence
- Having useless or unwanted product features can be another reason why projects fail. They add too many features and most of the them they are never or rarely used
	- Development teams would build software, *throw it over the wall* and then hope that some what they build would stick

### Requirements in Agile Software Development
### Agile Manifesto - Revisit
- **Individuals and interactions** over processes and tools
- **Working software** over comprehensive documentation
- **Customer collaboration** over contract negotiation
- **Responding to change** over following a plan
### Requirements in Agile Software Development
- Work continuously with stakeholders to develop requirements and tests
- Iterative development
	- Short iterations 2-4 weeks each focused on core features
	- Maintain working prototype while adding new features
	- Check with stakeholders what's next to validate building the right software
There is no "standard way" to do requirements in Agile development
- Normal lightweight "Software Requirements Document"
	- Agile is inherently adaptive and does not prescribe a single way to handle requirements
	- Requirement documentation is typically more lightweight and just detailed enough to facilitate development and testing without becoming a burden
- Based on **user stories**
	- Main vehicle for conveying requirements in Agile development
- Iteratively elaborate small set of the functionality requirements
	- In Agile, you don't need to have all requirements defined upfront and teams identifies and elaborates on requirements iteratively
	- A team may start with a high-level backlog of user stories and refine these progressively across iterations as a particular story gets closer to being developed
- Create some acceptance tests at the same time as your write the requirements
- 
## Behavior Driven Development
### Behavior-Drive Development (BDD)
- An Agile software development technique that encourages collaboration among developers, QA, and non-technical or business participants in a software project
- BDD brings together different stakeholders and define a feature that holds business value. It then outlines some specifications and see if the stakeholders accept those specifications and if they do, they developers can start coding it up
	- Business value (feature) -> Acceptance Criteria -> Code to deliver
- *Given-When-Then* canvas
	- Given a scenario; When the user does this action; Then what is to happen
	- Improves communication among domain experts, users, testers, and developers

### BDD - Requirements
- User stories to elicit functional requirements
- Low Fidelity User Interfaces (UIs) and Storyboards to elicit UIs
- Transfer user stories into acceptable tests

### BDD - User Stories
A short and concise description of how the application is expected to be used from the user's point of view
- Functionality that has value to the user / customer
- We need feature X but we do not have enough details about it
- We got more details about feature Y

BDD helps **stakeholders plan and prioritize developments** with **improvement in clarity** and **documentations for functional requirements as executable scenarios/examples**

### Agile Development - User Stories
- A common way to express very high-level requirements
- Universal in Scrum and other agile methods
- Short piece of text
	- 1-3 non-technical sentences written jointly by stakeholders and developers
	- Small enough to implement in one iteration, testable, and must have business value

A problem with user stories is that it helps build software that are likely never needed
- CHAOS reports that 45% and 19% of features were never used and rarely used respectively

### User Stories - Common Mistakes
- It talks aboutt a Generic user rather than a meaningful role
- No evident business value
	- Less technical details
		- "wouldn't it be cool if ..."

### SMART User Stories
- S - Specific
	- Describe specific details that add value
- M- Measurable
	- User stories should be testable; there are known expected results for some good inputs
- A - Achievable
	- Can be implemented in one Agile iteration
	- Subdivide big stories (features) into smaller ones
- R - relevant
	- Must have business value relevant to one or more stakeholders
	- Use "*Five Whys*" technique to help drill down to uncover real business value
- T - Timeboxed
	- Estimate the amount of time to implement it
	- Stop developing the story if allocated time is over
		- Divide into smaller ones or reschedule what's left to new estimate
		- If dividing doesn't help, discuss with the customer the part of highest value
![[Pasted image 20231003121631.png]]
### User Stories - "Done"
- An effective way for developers to gauge completion of features is to use the Acceptance Criteria
- This **Acceptance Criteria** is written at the back of the user story index card and consist of conditions which if fulfilled can be considered "Done"
![[Pasted image 20231003152155.png]]
### User Stories - Epics
- Larger bodies of work that can be broken into smaller tasks
	- Often compass multiple teams on multiple projects
	- Almost always delivered over a set of Sprints
- User stories short requirements or request written from the end user prespective
	- Who, what, and why
	- Defines details of an epic

### Scrum Artifacts - Product Backlog
- Features of sub-features needed to build the product
	- User stories, enhancements, and fixes identified from previous sprints
- Maintained by the **product owner** in collaboration with customers and team
- Source of product requirements
	- Items ordered by priority - value to the customer

### Scrum Events - Sprint Planning (Revisit)
- Identify Sprint Goal
- Identify work to be done to deliver this
- Two-parts meeting (SM, PO, and Dev team)
	- Before Meeting: PO prepares prioritized list of most valuable items
	- Part 1: PO & Dev team select items to be delievered at the end of sprint and on the team's estimate of effort
	- Part 2: Dev Team figure out the individual tasks they'll use to implement those items
- Output: Sprint backlog

### Scrum - User Stories
1. Break the defined stories down into tasks
	- By team members
	- Each task on a separate card
	- Uncompleted stories - put on a single card to plan out the story
### Planning and Running a Sprint
2. Group the stories and their task together
	- Add them to the "*To-Do*" of the Sprint Backlog
![[Pasted image 20231003152811.png]]
3. Team members to work on the tasks
	- Each team member works on one task at a time by moving it to the "*In-Progress*"
	- Team members can add additional tasks to the board to finish a story
		- Remember to communicate it in the *Daily Scrum*
![[Pasted image 20231003153001.png]]
1. Finishing a Task / Story
	- Task completed -> "Done" and pulls another task
	- Team members to finish the story's final task verifies that all the conditions of satisfaction are completed with the PO and move it to the "Done"
![[Pasted image 20231003153010.png]]
## User Interfaces, Scenarios, and Storyboard

### User Interface (UI) Sketches
- **Low-Fidelity (Lo-Fi) UIs**
	- Rough (UI) sketch or mock-up of user story
	- Low-teach approach to UIs and the paper prototype sketches
		- Pencil and paper
	- Show how a UI looks like and how sketches work together as a user interacts
	- Effective for engaging non-technical stakeholders
- **High-Fidelity (Hi-Fi) UIs**
	- Higher level of details that mote closely matches the design of the actual UI
		- Will also be used for documentation
### Lo-Fi Example
![[Pasted image 20231003153245.png]]

### Scenarios and Storyboards
- Scenario: Written discription of the system interactions from user's prespective
- Storyboard: similar to scenarios but it visualizes the interactions
- User story refer to a single feature

### Scenarios Format
```
Scenario: Brief description of scenario
Given: Description of context info or pre-condition
When: Description of action / event
Then: Description of outcome
```
![[Pasted image 20231003153725.png]]
### Searching the Movie Database - Scenario
![[Pasted image 20231003153745.png]]
### Implicit vs. Explicit Scenarios
- **Explicit Requirements** - Explicitly specified as user stories developed by stakeholders in BDD
  - **Implicit requirements** - Not specified in the user stories
  - Explicit requirements correspond to acceptance tests and implicit requirements correspond to integration tests
  - Tools such as *cucumber*, allows creating both acceptance and integration tests if user stories are written for both explicit and implicit requirements
### Imperative Scenarios
- Tend to have complicated WHEN statements and lots of AND steps
	- Ensures that UIs match customer expectations
	- Tedius to write such scenarios and not a good picture
- Scenarios should focus on the application behavior
### Declarative Scenarios
- Focuses on the feature being described by using the step definitions to make a domain language for the application
	- Domain language is informal but uses terms and concepts specific to your application rather than generic terms and concepts specific to the UI
- User stories should be written in domain language that you will have to develop via step definition for the application you build
- 
