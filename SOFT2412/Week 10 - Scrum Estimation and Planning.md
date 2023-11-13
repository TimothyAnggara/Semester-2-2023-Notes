## Plan And Document Software Development
![[Pasted image 20231010100339.png]]
### Plan and Document Software Methodologies
- The goal in Plan and Document methodologies is to make software engineering predicable in budget and schedule
	- Requirements elicitation
	- Requirements documentation
	- Cost Estimation
	- Scheduling and monitoring shcedule
	- Change management for requirements, cost, and schedule
	- Ensuring implementation matches requirement features
	- Risk analysis and management
![[Pasted image 20231010100324.png]]
### Requirements Documentation
- Software Requirement Specifications (SRS) process
	- 100s of pages, IEEE 830-1998 standard recommended practice for SRS
- Stakeholders read SRS document, build basic prototype, or generate test cases to check
	- Validity: Are all requirements necessary?
	- Consistency: Do requirements conflict?
	- Completeness: Are all requirements and constraints included?
	- Feasibility: Can requirements be implemented?
- Estimate budget and schedule based on the SRS
### Why Software Projects Fail?
- Software becomes overbudget or over-time
	- Hard to maintain and evolve
- Software has useless product features

## Planning in Agile Software Development
### Agile Manifesto - Planning
- Under the Agile manifesto, it is said that we should 
	- 'Respond to change over following a plan'
- And under the Agile Principles, these 4 points relate most to planning and estimation
	- Deliver Working software frequently, from a couple of weeks to a couple of months with a preference to the shorter timescale
	- Working software in the primary measure of progress
	- Agile process promote sustainable development, stakeholders should be able to maintain a constant pace indefinitely
	- At regular interval, the team reflects how to become more effective and adjust accordingly
### Planning in Software Development
There are two types:
- Plan-driven (plan-and-document or heavy-weight)
	- All of the process activities are planned in advanced and progressed is measured against the plan
	- Plan drives everything and change is expensive
- Agile process (light-weight)
	- Planning is incremental and continual as software is developed
	- Easier to change to reflect changing requirements

## Agile Methods for Estimating Size
### Estimating Size in Agile Development
 - Agile teams separate estimates of size from estimates of duration
![[Pasted image 20231010102724.png]]
### Recall: Requirements in Agile Software Development
- When creating requirements for Agile Development, we work continuously and closely with stakeholders to develop requirements and tests
- **Iterative development**
	- Short iteration: 2-4 weeks each focused on core features
	- Maintain a working prototype while adding new features
	- Check with stakeholders to validate what to build next
	- Initial planning and estimation and adapt during development
### User Stories - Basis for Better Estimation and Monitoring
- Recall that user stories is a informal general explanation of a software feature and user stories have a condition of satisfaction written at the back
![[Pasted image 20231010103336.png]]
### Estimation - Story Points
- Story point is a metric for the overall size of a user story, feature, or other piece of work
- A point value is assigned to each item
- The number of story points --> overall size of the story
- The estimation scale used for these story points follow either:
	- The Fibonacci series
		- 1, 2, 3, 5, 8, ...
	- Multiplying twice the number before it;
		- 1, 2, 4, 8, …
- Why do we use story points?
	- Measure of the user story only
	- No emotional measurements
	- Team velocity is considered
	- Team members can focus on solving problem based on difficulty and not on time spent
#### Estimation - Staring Story Points
- There are two approaches for assigning story points:
	- Smallest story is estimated at 1 story point
	- Medium-sized story is assigned a number somewhere in the middle of the range you expect to use
- Estimating story points completely separates the estimation of effort from the estimation of duration

### Sprint Planning Session Using Story points
- Start with the most valuable user stories from the product backlog
- Take a story in that list (ideally the smallest one)
- Discuss with the team whether that estimate is correct
- Keep doing through stories until the team have accumulated enough points to fill the sprint
#### Why do Story Points Work?
- They are simple 
- Team is in control
- Gets your team to start talking about estimates
- Developers are not scared of them
- Help the team discover what exactly a story means
- Helps everyone on the team become genuinely committed

### Ideal Days vs Elapsed Days
- Ideal time and elapsed time are different
	- An American football game is 60 minutes however, 3 or more hours will typically pass on a clock before a 60 minute game is finished
- Time to do a development task without any interruptions is?
- Time to do a development task with work interruptions is?

#### Estimating in Ideal Days
- Ideal Days - Amount of time a user story will take to develop
	- Only an estimate of size
- Elapsed Days - Requires considering all interruptions that may occur while working on a user story
- When estimating, its important to associate a single estimate with each user story
	- Not in four programmer days, two tester days, three product owner days, etc.
	- Express the estimate as a whole
		- 9 ideal days
### Estimation Techniques
- Expert opinion
- Analogy: Compare with other stories
- Disaggregation: Split a story into smaller task
- Planning Poker: Based on an expert opinion, analogy, disaggregation, and fun

#### Estimation Technique - Planning Poker
- A gamified technique to estimate effort / relative size of development in Agile Development
- Best way for agile teams to estimate is by playing planning poker
	- Grenning 2002
- Aims to avoid individual influence/bias
- Team discussion with mediation to consider different views
- Team involvement in planning increases commitment
- Combines expert opinion, analogy, disaggregation into an enjoyable approach to estimating that results in quick but reliable estimates

### Considering Story points over Ideal Days
- Story points help drive cross-functional behavior and it has a pure measure of size
- Estimating in story point in typically faster and it can consider that each person have different values for ideal days
#### Re-Estimating
- Story points and ideal days helps you know when to re-estimate
- Important that you only Re-estimate when opinion of the relative size of one or more stories has change
	- Not because progress is not coming as rapidly as you'd expect
- Let velocity take care of most estimation inaccuracies

### Considering Ideal Days over Story points
- Ideal days are easier to explain outside the team
- Ideal days are easier to estimate at first
- Ideal days make velocity predictions easier

## Estimating Progress

### Estimating Progress - Velocity
- Velocity is the measurement for the team's rate of progress
	- Sum the number of story points assigned to each user story that the team completed during the iteration
		- Team completed 3 stories, 5SPs each --> $3 \times 5 =15$
### Estimation - Number of Sprints
- To find the size estimate of the project:
	- Sum the Story points estimated for all desired features
- To find the estimated number of Sprints
	- Divide the size by velocity to arrive at an estimated number of sprints
#### Example
- Sum of all user stories is 100 story points
- Team's velocity, based on past experiences, is 11 SP every two week sprint

- How many sprints?
	- 100 / 11 = 9.1 iterations
	- Lets round up to 10
- Estimate of duration?
	- 10 sprints * 2 weeks = 20 weeks
- Project Schedule
	- Count forward 20 weeks on the calendar and that becomes our schedule

### Estimating Velocity
- Use Historical Values
- Run a Sprint
- Make a forecast
#### Consideration for Historical Values
- Is the technology the same?
- Is the domain the same?
- Is the team the same?
- Is the product owner the same?
- Are the tools the same?
- Is the working environment the same?
- Were the estimates made by the same people?
#### Run A Sprint
- Run a couple of sprints and then estimate velocity from the observed velocity during one to three iterations
#### Forecasting
- Estimate the number of hours each person will be available to work on the project each day
- Determine the total number of hours that will be spent on the project during iteration
- Somewhat randomly select stories and expand them into their basic tasks
- Repeat until you have identified enough tasks to fill the number of hours in the iteration
- Covert velocity determined in prior step into a range

### Velocity - Planning and Estimation
- Inconsistent velocity over long time
	- Hidden challenges not counted?
	- Outside business/stakeholders' pressure
- Observe team velocity throughout the Sprints and investigate decrease in average velocity
	- Discuss in the retrospective meetings

## Tracking Project Progress
### Revisit - Planning and Running a Sprint using Stories, Points, Tasks, and a Task Board
- First half of the Sprint planning
	- Story points and velocity to figure out what will go into the Sprint
- Second half of the Sprint Planning
	- Plan out the actual work for the team is to add cards for individual tasks
	- Tasks can bet written code, create design and architecture, etc.
![[Pasted image 20231010111748.png]]
### Burndown Charts
- Tracks completion of development work throughout the Sprint
- Should be visible to everyone in the team
- First half of the Sprint planning
	- Story points and velocity to figure out what will go into the Sprint
- Good estimation and planning should help the team to burn stories relatively with similar pace
#### Burndown Charts based on Story Points
- x-axis: first and end dates of the sprint
- y-axis story points
- A user story is 'done', plot t he number of points left in the sprint at the current day
- When more works needs to be added when discovered during daily scrum
	- Estimate amount of points to remove to balance out the Sprint
	- Add card(s) to the task board and follow-up team meetings to estimate added work
	- Add notes to the chart
- As you get close to the end of the Sprint, more points burn off the chart
![[Pasted image 20231010112057.png]]
