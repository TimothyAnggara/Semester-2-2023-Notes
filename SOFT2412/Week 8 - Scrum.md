## Agile Principles
1. Highest Priority to satisfy customer through continuous delivery
2. Welcomes changing requirements
3. Deliver working software frequently
4. Stakeholders and developers must work together throughout the project
5. Build projects around motivated individuals and give the individuals the environment and support they need with trust
6. Most efficient and effective method of conveying information is face-to-face conversation
7. Working software is the primary measure of progress
8. Agile processes promote sustainable development and should be able to maintain a constant pace indefinitely
9. Continuous attention to best practices
10. Simplicity - maximizing the amount of work not done is essential
11. Best architectures, requirements, and designs emerge from **self-organizing teams**
12. At regular intervals, team meetings to reflect how to adjust and tune behavior to be more effective

### Agile Methods
All have a shared goal which is delivering valuable software iteratively but they may have some different practices to achieve this main goal
- How coding should be done, how work is arranged, team structure, and communication

Example of Agile methods:
- Scrum
- Extreme Programming (XP)
- Lean software development
- Kanban (lean development)
## eXtreme Programming (XP)
- development and delivery of **VERY SMALL INCREMENTS** of functionality
- Relies on constant code improvement 
- Emphasizes **Test Driven Development** as part of the small development iterations

#### XP Practices
- Customer team member
- User stories 
- Acceptance Test
- Pair Programming
- Test-Driven Development
- Short Cycles
- Continuous Integration
- Simple desisg
- Refactoring
- Collective ownership

### Scrum
Originating from a "product development" lifecycle from H. Takeuchi and I. Nonaka (1986) where it applies a new approach to increase speed and flexibility
- Used for software development by K. Schwaber, J. Sutherland, and many others
- Extensively used in various organizations
	- Manufacturing, product development, software, hardware, etc.
- An Agile method for managing software development focused on delivering products of the highest value
- Focuses on continuous improvement of the product, team, and working environment
- Lightweight, simple to understand but difficult to master
#### Scrum Theory
Based on an '**empirical process control**' theory where knowledge comes from experience and decisions are based knowns
- An iterative, incremental process which aims to optimize predictability and control risks
##### Pillars of Empirical Process Control
- Transparency - Significant aspects of the process must be visible to those responsible for the outcome
- Inspection - Scrum users must frequently inspect Scrum artifacts and progress toward an iteration goal to detect undesirable variances
- Adaptation - Adjust the aspects of the process that lead to deviation outside the acceptable limits
#### Scrum Values
- Commitment - Personally commit to achieving the goals of the Scrum Team
- Courage - members can do the right thing and work on tough problems
- Focus - Everyone focuses on the work of the iteration and the team's goals
- Openness - Team and stakeholders agree to be open about their work and challenges with performing the work
- Respect - team members should respect each other to be capable and independent people

#### Scrum Practices
- Teams and their roles
	- Product owner, Scrum master, Dev team
- Events
	- Sprints, Sprint planning, Daily scrum, Sprint review, and Retrospective
- Aritfacts
	- Product backlog, Sprint backlog, Increment
- Project estimation and Sprint estimation
- Rules govern the relationship between roles, events, and artifacts
- All of the above components are essential for Scrum Success

## Scrum Team
- Team Roles
	- Development Team
	- Product Owner (one person)
	- Scrum Master (one person)
- Scrum Team
	- Small enough to be agile
	- Cross-functional
	- Self-organizing
	- Deliver products iteratively and incrementally
		- Maximizing opportunities for feedback
#### Product Owner
A person that maximize the value of the product and the work
- Talks to customers, understands requirements and the priorities
- Managing the Product Backlog (One Person)
	- Records product backlog items and order it
	- Optimize the value of the work the development team performs
	- Ensure transparency and clarity of the product backlog
	- Ensure development team understands product backlog
	- Can assign it to the development but still accountable
#### Development Team
Professions who work on delivering the potentially releasable product at the end of each iteration
- Creates the increment
Development team sizes are generally either
- Less than 3 members
- More than 9 members
The professionals of this team are **self-organizing, cross-functional, without sub-teams** and the **whole team** is accountable if something goes wrong
- self-organizing - Turns backlog into increments of potentially releasable functionality
- Cross-functional - has a mix of skills necessary to create the product
#### Scrum Master
The person who is the scrum masters serves both the product owner and the dev team
- In context of serving the product owner:
	- Creates mutual understanding of goals, scope, and product domain
	- Finding effective ways to manage the product backlog
	- Helps the Scrum team understand the need for clear and concise product backlog items
	- Ensures the product owner knows how to arrange the product backlog to maximize value
	- Understands and practices agility
	- Facilitating scrum events when needed
- In context of serving the Dev team
	- Become self-organizing and have cross-functionality
	- Create high-value products
	- Removes impediments to the Dev team's progress
	- Facilitates Scrum events
	- Coaches the Dev Team

## Scrum Events
- Used to create regularity and minimize the need for meetings
- Time-boxed
- Cannot be changed once an iteration has started
- Designed to enable transparency and to provide a formal opportunity to inspect and adapt work
### The Sprint
- Development iteration
	- Usable and potentially releasable product increment is created
- Time-boxed (2-4 weeks)
	- Long sprints may lead to changes in the definition
- Sprints have consistent duration during product development
- Consist of:
	- Sprint planning
	- Daily Scrum
	- Development Work
	- Sprint Review
	- Sprint Retrospective
#### Sprint Planning
- Identify the sprint goal
	- Items from the Product Backlog
- Identify work to be done to deliver this
- Two-part meeting
	- **Before Meeting**: Product Owner prepares prioritized list of most valuable items
	- **Meeting 1** (max 4 hours): Product Owner & Dev Team select items to be delievered at the end of the sprint based on their value and on the team's estimate on how much work needed
	- **Meeting 2** (Max 4 hours): dev team figure out tasks they'll use to implement those items
#### Scrum Iteration Process
- A Sprint(Scrum Iteration) contains a list of tasks and work product outputs that will be done in defined duration
	- Beginning of a Sprint duration, each team member has a pretty good idea of what they'll be working on
	- Management should not add new work product outputs to the Sprint 
		- Should be added to the Product Backlog instead
![[Pasted image 20230919130626.png]]
#### During Sprint
- **Daily Scrum Meeting**
	- Ensure problems and obstacles are visible to the team
	- Timeboxed to 15 minutes
	- All team members including Scrum Master and Product Owner must attend
		- Stakeholders may attend as observers
	- Answer three question:
		- What did I do yesterday that helped the development team meet the Sprint Goal? 
		- What will I do today to help the development team meet the Sprint Goal? 
		- do I see any obstacles that prevent me or the Dev team to meeting the Sprint Goal
	- No problem-solving during meeting
		- Follow-up meetings if further discussion is required
- **Development Work**
	- Builds the items in the Sprint backlog into working software
	- Inform product owner if they are overcommitted or can add extra items if time allows
	- Update sprint backlog and keep it visible to everyone
- **Sprint Review**
	- Informal meeting at the end of sprint (4 hours)
		- Dev team demonstrates working software to customers / stakeholders
		- Stakeholders share feedback
		- Product owner explains "Done" and not "Done" items
	- Output - Revised Product Backlog and Probable items for next iteration
#### Retrospective
- **Retrospective Meetings**
- Scrum Master and Dev. team (maybe product owner) will answer two questions:
	- What went well during the Sprint?
	- What can be improved in the future
- Scrum master notes improvements that should be added to the product backlog
	- Better build server, Adopt design principles, Change office layout
- Output - identified improvements to be implemented in the next Sprint

## Scrum Artifacts
### Product Backlog
- Set of all features and sub-features needed to build the product
	- Features, functional, requirements, enhancements, fixes identified from previous Sprints
- Maintained by Product Owner in collaboration with customer and teams
- Source of the product requirements
	- Evolves over time and never complete (dynamic)
	- items ordered by priority 
		- Priority is determined by customer
- A simple spreadsheet with items called "customer features"
	- User screen, interaction scenario, use case, new algorithm, etc.
- Some items are **internal tasks** that contribute to the value of the product
- Effort Estimates - assigned to each item by the Dev Team

### Sprint Backlog
- Set of product backlog items selected for the sprint with a plan delivering the product increment and realize the Sprint Goal
- Dev. Team forecasts next items to be implmented
- Includes at least one high-priority improvement identified from previous Sprint
- Dev. team adds new work to the Spring Backlog
- Estimated remaining work is updated once an item is complete
- Visible to anyone and to be modified by the Dev. Team
![[Pasted image 20230919131915.png]]
### The Increment
- Collection of Product Backlog items that meet the "Done" definition
- "Done" - Team's shared agreement on the criteria that a item must meet before considered Done
	- Work will not be counted toward the end of Sprint if it does not beet this criteria
## Scrum Estimation
### Progress Monitoring
- Total work remaining to each the goal
	- Product Owner tracks this at least every Sprint Review
	- Compares with previous Sprints to assess progress towards projected work
	- Forecasting progress through burn-downs, burn-ups, or cumulative flow
	- Note that this is just an estimate
- "Burndown" charts tracks the amount of estimated effort remaining in a Sprint
	- Maintained on a daily basis by the Scrum Master
- Velocity tracks the amount of work from Sprint to Sprint
	- Velocity chart graphically shows project/team's velocity
![[Pasted image 20230919132505.png]]
![[Pasted image 20230919132512.png]]

