## LO:
- Agile Development
- Continuous Integration (CI)
- Effective CI
	- Automation practices
	- Team Practices
- Continuous Delivery
- Continuous Deployment
- Tooling - Jenkins

## Agile Development
### Revisit
In agile development, the aim is to satisfy the customer through early and continuous delivery of valuable software. It welcomes changing requirements and delivers working software frequently
- Working software is the measure of progress

### What is Continuous Integration

Continuous Integration is a software development practice where team members integrate their work frequently with each person integrates at least daily. Each integration is verified by an automated build to detect errors as quickly as possible

Imagine two developers working on two separate components, A and B. They both finished their work and the components needs to be integrated and verified such that the system works as expected
- The developers may work on components integrated into subsystems
- The subsystems may be integrated with other team's subsystem to form a larger system

The goal of continuous integration is to ensure that software is always in a working state
- Whenever changes are made, a set of automated tests are run against it to ensure that it works
- If the build or test fails, the development team should stop what they're doing and fix the problem immediately


The objective of CI is to minimize the duration and effort requirement by each integration and also be able to deliever product version suitable for release at any moment
- To achieve these objectives:
	- Integration procedure which is reproducible
	- Largely automated integration

With continuous integration, bugs are caught much earlier in the delivery process when they are cheaper to fix thus providing significant cost and time savings

### Continuous Integration and Tooling
CI Should not be confused with the tools that assist it
- CI is a development practice not a tool
- Different automation tools can be used such as version control, system build, and testing for the CI development practice
- CI should lessen the pain of integration by increasing its frequency

To implement continuous integration, we can use many different tools such as:
- Version Control
- Automated build process
- Commitment and discipline from development teams
- Configuration of the system build and testing processes
- Use of CI server to automate the process of integration, testing, and reporting of test results

## Continuous Integration Workflow
![[Pasted image 20230905110137.png]]
1. A developers check-out the system mainline from the VC system
2. Build the system and runs automated test to ensure the build system passes
	- If test were not passed, notify the developer who check-in the last baseline of the system
3. Make changes to the system components
4. Build the system in a private workspace and rerun system tests
5. System passed test and check it into the build system server
6. Build the system on the build server and run the tests
7. Commit the changes they have made in the system main line

## Effective Continuous Integration - Automation Practices
### Regular Check-in
Developers check in their code into the mainline regularly (A couple times a day)
- Small changes less likely to break the build
- Easier to revert back to the last committed version
Check-in vs Branching
- Working on a branch means your code is not being integrated with other developer's code, especially with long-lived branches

### Create a comprehensive automated test suite
There are 3 types of test that should be made inside a CI build:
- Unit Testing - Test the behavior of small pieces in isolation
- Component Testing - Test the behavior of several components
- Acceptance Testing: applications meets the acceptance criteria set by end users (functional and non-functional)
The test made should provide a high level of confidence that the changes made will not break any existing functionality

### Keep the build and test process short
We should aim our build process to be 1.5 to 10 minutes and make sure that the tests we are running is optimized to achieve at much coverage as possible.
To make the build process short, we should split testing into two stages:
- Compile software and run suite of unit tests and create deployable binary
- Use binary to run acceptance, integration, and performance tests

We want to keep our build and test process short because:
- Developers may stop doing full build and running test before they check-in
- CI will likely miss multiple commits if it takes too long
- Leads to less check-ins as developers wait for the software to build and tests to run

### Developers to manage their development workspace
Developers should be able to build, run automated test, and deploy the software on their local machines using the same processes used in CI

### Use CI Software
CI activities often has 2 components:
- CI Workflow execution
	- It polls VCS, check out a copy of the project to a directory on the server if it detects changes
	- Executes commands to build application and run automated tests
- CI results visualization and reporting
	- Results of the processes that have been run, notifies you of the success or failure of the build and automated tests and provide access to test reports

## Effective Continuous Integration - Essential Practices - Development Teams

### Don't check in on a broken build
- If the build fails, responsible developers will need to find the cause of breakage and fix it quickly
- CI process will always identify such breakage and will likely lead to interrupting other developers in the team
### Always run all commit test locally, or get your CI server to do it for you
???

### Wait for commit Tests to pass before moving on
Continuous integration is shared among all develops so to ensure that the system is consistent and stable, let all the tests run and fix any issues without interrupting the workflow

### Never go home on a broken Build
- Check in regularly and early enough to give yourself time to deal with problems
- If the build it broken, revert changes until it is fixed but leave the changes on your local working copy

### Always be prepared to revert to the previous revision
- Fix it quickly or revert and fix it locally
- One of the VCS benefits
- Mindset - be prepared

### Time-box fixing before reverting
Set a team rule that if a failure occurs, fix it in X-minutes or revert it otherwise

### Do not comment out failing test
- Often a result of time-box fixing
- It takes time to find the root cause of a failing test

### Take responsibility for all breakages that result from your changes
- It is your responsibility because you made the change, to fix all tests that are not passing as a result of your change
- To fix breakages, developers should have access to any code that they can break through their changes

## Continuous Delivery

Continuous delivery is a software development discipline where you build software in such a way that the software can be released to production at any time
- Release new changes to customers quickly and in reproducible way
- Automate the release process so you can deploy your application at any time
	- Package artefacts to be delievered to end users

### Why Continuous Delivery
- Cycle times becomes hours not months
- Quality is built-in to the process
- Late feedback is expensive

### Deployment Pipeline
The deployment pipeline for a continuous delivery practice uses an automated implmentation of the application's build, deploy, test, and release process
- Its uses the CI process as its foundations
![[Pasted image 20230905123751.png]]
![[Pasted image 20230905123801.png]]

## Continuous Deployment

### Continuous Delivery Vs Continuous Deployment
![[Pasted image 20230905123935.png]]
### Deployment Automation
- Deployments should tend towards being fully automated
	- Pick version & environment
	- Press "Deploy" button
- Automated deployment scripts should be up-to-date
- Don't depend on the deployment expert
- Automated deployment process:
	- Cheap and easy to test
	- Fully auditable
	- Should be the only way which the software is ever deployed

## Continuous Integration / Delivery Tools
### Jenkins
- Automations server to automate tasks related to building, testing, and delivering or developing software
- Jenkins pipeline supports implementing and integrating CD pipelines into Jenkins
	- A CD pipeline is an automated expression of your delivery process
	- Written in Jenkinsfile

#### Running Multiple Steps
- Building, testing, and deploying activities are defined as stages and steps
- Jenkins allow composing different steps (commands) to model simple and complex automation processes
- Special steps:
	- Retry: retrying steps a number of times
	- Timeout: exiting if step takes a long time
	- Clean-up: run clean-up steps or perform post action based on the outcome of the pipeline when it finishes executing
#### Execution Environment
- The **Agent** directive specifies how to execute pipeline
	- All steps contained within the block are queued for execution
	- A workspace is allocated which will contain files checked out from source control and any additional files for the pipeline
- Pipeline is designed to use Docker images and containers to run inside
	- No need to configure various tools and dependencies on agents manually

#### Recording Tests and Artefacts
- Junit is built in with Jenkins
- Jenkins can record and aggregate all test results and artefacts
	- Reported through using the post section
	- A pipeline with failing tests are marked as unstable
- Jenkins store files generated during the execution of the pipeline
	- Useful for analyzing and investigation in case of test failures

#### Cleaning and Notifications
- Post sections can be used to specify clean up tasks, finalization, or notifications
	- Post section will run at the end of the pipeline executions
- Notifications can be setup to
	- Send emails
	- Post on Slack or other
- Notifications can set-up when things are failing, unstable, or succeeding