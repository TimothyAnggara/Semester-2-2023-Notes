## Learning Objectives
- Software Quality Assurance
- Software Testing
	- Why, what, and how?
	- Testing level and techniques
	- Test cases design
	- Code (Test) Coverage
- Unit Testing Framework
	- JUnit
- Code Coverage Tools

## Software Quality Assurance
Software quality is being able to create a software that satisfies end use's needs with correct behavior while being easy to use and reliable
- Basically it runs as intended without any issues

Software quality assurance is ensuring that the software being developed is reaching organizational standards and has qualities of a good software quality. 
- Software quality is often determined through testing

### Software Testing - Cost
Software development and maintaining the software cost money and that includes testing as well. A study done about software testing on the US economy estimated that total cost of inadequate software testing costed the US $60 billion.

The study conducted also suggested that $\frac{1}{3}$ could have been avoiding with better software testing
- $30 Billion could have been saved!

Building quality software is not just about being able to pass test cases, it's making sure what has been built is functional, robust, and reliable
- Ability to deal with unexpected inputs, conditions, reliable, and be able to be developed in the future

There can be many dangerous side-effects of not developing software system where reliability could be a issue
- Medical devices: A bug could result in incorrect treatment
- Flight Control: Software errors could lead to aviation accidents
- Traffic Control: Glitches could lead to accidents on roads or in public transport systems

But beyond safety, satisfying user needs and resolving real-world problems is also an expectation of building quality software. Users can rely on the software and operate them without any doubts. 
- Small software errors could lead to disasters
![[Pasted image 20230829102425.png]]

## What is Software Testing?
Software testing is the process to demonstrate that the software meets its requirements and finding incorrect or undesired behavior caused by defects/bugs
- System crashes, incorrect computation, data corruption, unnecessary interactions

Software also tests different system properties such as Functional and Non-functional:
- Functional - Performs functions as expected
- Non-functional - Performance, secure, usability

### Testing Objectives
During the testing process, it is important for objectives of the testing to be defined as this means that we can better monitor and control the testing process ensuring that it meets the desired standards

Testing for completeness is also a very common conundrum. It is impossible to say that the testing is "complete" as there are an infinite many inputs and conditions, but it is important to be able to have as many test cases as possible but exhaustive testing is very expensive
- Better to use a risk-driven or risk management strategy for test for better testing
	- Deal with highest-risk elements then go down the risk 

Now, how much testing is enough then since we can't test for completeness? It's best to select test cases that are sufficient for a specific purpose.
- We can use Coverage criteria and graph theories to analyze test effectiveness

### Tests Modeling
Testing is modelled as input test data and output test data. There are two types of tests:
- Defective Testing - Test that cause defects/problems
- Validation Testing - Test that lead to expected correct behavior
![[Pasted image 20230829104014.png]]

### Who does the Testing?
There are many kinds of scenarios where who does the testing but there are some general people that does it.
- Developers test their own code
- Developers in a team test one another's code
- Specialist tester role
- Real users, doing real work

### Testing Takes Creativity
Being able to develop an effective test, it needs a deep understanding of the system, knowledge of testing techniques, and some domain knowledge which will allow testers to craft test that reflect real-world scenarios

Testing is done best by independent testers because as developers we often develop a certain mental attitude that the program should be a certain when in fact it does not.
- Programmers often stick to the data set that makes the program work
- Sometimes the program may not work when being used by somebody else


### When does Testing take Place?
In the waterfall software Development framework testing occurs during the Verification stage
![[Pasted image 20230829105029.png]]

In the Agile Software Development framework testing occurs during the integration phase but (I think testing occurs throughout the stages)

![[Pasted image 20230829105114.png]]
### Software Testing Process
The foundation of software testing process involves **designing** a structured plan for testing, **executing** it, and **managing** all test-related activities thus ensuring that testing is organized and methodical
- We must carefully select and prepare test cases which we think are suitable for certain scenarios and inputs. Tests should also select suitable testing techniques which include black-box, white-box, regression, etc.
- Once the test plans are designed, we execute the tests and observe any discrepancies or defects and if they do, we should understand the root cause of the problem and solve them
- Like any process, testing comes with constrains which mean they will make trade-offs between schedule, available resources, desired test coverage, or adequacy

It is also crucial to assess how well the test are performing thus we are interesting at their effectiveness and their efficeincy
- How well they identify issues and how fast and resource effective they are
- The testing process is shaped by several factors:
	  - Available resource, tools, environment, budgets, etc.
	  - Schedule
	  - Skills and knowledge of team members
- A significant factor in the ease and effectiveness is if the developer has testing in mind to make testing much easier

![[Pasted image 20230829105329.png]]
### Types of Defects in Software
- Syntax Errors
	- Picked up by IDE
- Runtime Errors
	- Crash during execution
- Logic Error
	- Does not crash but output is not what the specs asks it to be
- Timing Error
	- Does not deliver computational result on time

## Software Testing Levels
### Testing Levels
| Testing Level             | Description |
| ------------------------- | ----------- |
| Unit / Functional testing | Process of verifying functionalities of software components independently from the whole systems. (Functions, Components, etc.)            |
| Integration Testing       | Process of verifying interactions / communications among software components. Incremental integration testing vs. "Big Bang" testing            |
| System Testing            | Process of verifying functionality and behaviour of the entire software system including security, performance, reliability, and external interfaces to other applicatioons            |
| Acceptance Testing                          | Process of verifying desired acceptance criteria are met in the system (Functional and non-functional) from the user POV            |

### Integration Testing
- The process of verifying interactions / communications among software components behavior according to its specifications
- We do integration testing because software components may not behave correctly when they interact with each other
- We run high-level tests in Integration testing
![[Pasted image 20230830003125.png]]
### Regression Testing
- Verifies that a software behavior has not changed by incremental changes to software
- Modern software development process iterative/incremental steps which may introduce changes that may affect the validity of previous test
- Regression test is used to verify:
	- No new bugs are introduced
	- Pre-tested functionality still works as expected

## Software Testing Techniques
### Principle Testing Techniques
- Black Box Testing
	- No programming and software knowledge
	- Carried by software testers
	- Acceptance and system testing
		- High level tests
- White Box Testing
	- Programming and software knowledge
	- Carried by software developers
	- Unit and integration testing 
		- Lower level tests

### Test-Driven Development
- A particular aspect of many (but not all) agile methodologies
- We write tests before writing code which results in only writing code which will enable to pass the test
![[Pasted image 20230830003458.png]]
## Test Cases Design
- Partitioning Testing
	- Identify groups of input with common characteristics
	- For each partition, chose tests on the boundaries and close to the midpoint
- Guideline-based testing
	- Use testing guidelines based on previous experience of the kinds of errors often made
	- Understanding developers thinking
### Equivalence Partitioning

## Code (Test) Coverage
The extend to which source code has been executed by a set of tests
- Usually measured as a percentage, e.g. 70% coverage
- Uses different coverage criteria to measure coverage
### Coverage Criteria
| Coverage Criteria             | Description |
| ----------------------------- | ----------- |
| Method                        |             How many methods are called during the test|
| Statement                     |             How many statements are exercised during the test|
| Branch                        |             How many branches have been exercised during the test|
| Condition                     |           Has each separate condition within each branch been evaluated to be both true and false  |
| Condition / decision coverage |       Requires both decision and condition coverage be satisfied      |
| Loop                              |           Each loop executed zero times, once, and more than once  |

### Coverage Target
So what coverage should we aim for for our software? well, it critically determines coverage level. 
- If we are developing a software such as medical devices or aerospace software, we should aim for a very high coverage target
- If we are developing a software such as a game or a quality of life software, we should aim for a good coverage


## Unit Testing
### Some Terminologies
- Code under test - A specific portion of code being targeted for testing, specifically a method or class
- Unit test - Code segment created by a developer to run a particular function within the code under test and validate specific behaviors or states
- Test Fixture - Shared set of testing data and methods that prepare and configure this data for testing. Helps ensure reliable and reproducible results across multiple test cases

## Test Frameworks
Test frameworks are tools in software development that streamline the process of designing, executing, and managing test cases. They provide a structured environment to enable automatic execution of tests
- Automation allows frequent tests of code changes
- Enhancing software reliability and stability

Test frameworks also have easy to understand reports with a "Big Green" signal if all test pass successfully or a "Red" signal if any tests fail

### Unit Testing Frameworks - JUnit
JUnit is an open source framework for writing and running test for Java. IT uses annotations to identify methods that specify a test and can be integrated with build automation tools and Eclipse

### JUnit - Constructs
- JUnit test (Test class)
	- A method contained in a class which is only used for testing
- Test suite
	- Contains several test classes which will be executed all in the specified order
- Test Annotations
	- To define/denote test methods (E.g., @Test, @Before)
	- These methods execute the code under test
- Assertion methods (assert)
	- To check an expected result vs. actual result
	- Variety of methods
	- Provide meaningful messages in assert statements

### JUnit - Annotations
| JUnit 4             | Description |
| ------------------- | ----------- |
| import org.junit.** | Import statement to use the annotations            |
| @Test               |       Identifies a method as a test method      |
| @Before             |     Executed before each test to prepare the test environment        |
| @After              |   Executed after each test to cleanup the test environment and save memory          |
| @BeforeClass        |        Executed once, before the start of all tests to perform time intensive activities e.g. connect to a database     |
| @AfterClass                    |  executed once after all test have been finished to perform clean-up activities e.g., disconnect from a database            |

### JUnit Test - Example
```
public class MyClass{
	public int multiply(int n, int m){
		return n * m
	}
}
```
```
import static org.junit.Assert.assertEquals;
import org.junit.Test;

public class MyClassTests{
	@Test
	public void multiplicationTest(){
		MyClass tester = new MyClass();
		assertEquals(0, tester.multiply(10,0))
		assertEquals(0, tester.multiply(0,10))
	}
}
```

### JUnit - Assertions
The assert class provides static methods to test for certain conditions. It compares the actual value returned by a test to the expected value
- It allows you to specify the expected and actually results and the error message
- Throws an AssertionException if the comparison fails

### JUnit - Methods to Assert Test Results
| Method / Statement                             | Description |
| ---------------------------------------------- | ----------- |
| assertTrue(Boolean Condition, message)         |        Check that the Boolean condition is true     |
| assertFalse(Boolean Condition, message)        |      Check that the Boolean condition is false       |
| assertEquals(expected, actual, delta, message) |        Tests that the the values are the same, Note: for arrays, the reference is checked, not the content     |
| assertNull(object, message)                    |       Test that float values are equals within a given delta      |
| assertNotNull(object, message)                                           |         Checks that the object is null     |

### JUnit - Static import
```
// Without Static import
Assert.assertEquals(50, tester.multiply(10,5), "message")

//With static import
import static org.junit.Assert.assertEquals;
assertEquals(50, tester.multiply(10.5),"message")
```

### JUnit - Executing Tests
- We can run JUnit test from the command line and then run `runClass()` method provided by the `org.junit.runner.JUnitCore` which will allow developers to run one or more multiple test classes
- 

### JUnit - Executing Tests from Command Line
1. Compile JUnit tests in command line
	`javac -cp junit-4.12.jar;. UserDAOTest.java ProductDAOTest.java`
2. Run JUnit tests in command line
	`java -cp junit-4.12.jar;hamcrest-core-1.3.jar;. org.junit.runner.JUnitCore UserDAOTest ProductDAOTest`

### JUnit - Test Execution Order
JUnit assumes that all test method can be executed in an arbitrary order
- Good test code should not depend on other tests and should be well defined 
You can control test execution order but it will lead into problems
- Poor test practices
By default, JUnit 4.11 uses a deterministic order
- We can use @FixMethodOrder to change test execution order
	- Not recommend
- @FixMethodOrder(MethodSorters.JVM)
- @FixMethod Order(MethodSorters.NAME ASCENDING)

### JUnit - Parameterized Test
- A class that contains a test method and that test method is executed with different parameters provided
- Marked with @RunWith(Paramtereized.class) annotation
- Test class must contain a static method annotated with @Parameters 
	- Method generates and returns a collection of arrays, each item in this collection is used as a parameter for the test method

### JUnit - Verify Tests Timeout Behavior
- To Automatically fail test that take too long
- Timeout parameter on @TEst
	- Causes test method to fail if the test runs longer than the specified timeout
	- Timeout in milliseconds in @Test
```
@Test(timeout = 1000)
public void testWithTimeout(){
	...
}
```

### JUnit - Rules
- A way to add or redefine the behavior of each test method in a test class
	- E.g. Specify the exception message you expect during the execution of test code
- We can annotate fields with @Rule
- JUnit already implements some useful base rules

| Rule              | Description |
| ----------------- | ----------- |
| TemporaryFolder   |             Creates files and folders that are deleted when the test finishes|
| ErrrorCollector   |             Let execution of test to continue after first problem is found|
| ExpectedException |         Allows in-test specification of expected exception types and messages    |
| TimeOut           |         Applies the same timeout to all test methods in a class    |
| ExternalResources |     Base class for rules that setup an external resource before a test        |
| RuleChain                  |         Allows ordering of TestRules    |

```
public static class UsesErrorCollectorTwice{

	@Rule
	public final ErrorCollector collector = new ErrorCollector();

	@Test
	public void example(){
		collector.addError(new Throwable("First thing went wrong"))
		collector.add(new Throwable("Second thing went wrong"))
	}
}
```

### JUnit - Eclipse Support
With Eclipse, we can create JUnit tests via the Eclipse wizard or we can also just write them manually.
- Eclipse IDE also supports executing tests interactively
	- Run-as JUnit Test will start Junit and execute all test methods in the selected class
- Eclipse can also be used to extract the failed test and stack traces and also to create test suites

### Tests Automation
To use JUnit in your Gradle build, we can add a testCompile dependency to your build file. Gradle adds the test task to the build and needs only appropriate JUnit JAR to be added to the classpath to fully activate the test execution

## Code (Test) Coverage Tools
### Tools for Code Coverage in Java
There are many tools/plug-ins for code coverage in Java such as EclEmma. EclEmma is a code coverage plug-in for Eclipse
- Provides the rich features for code coverage analysis in Eclipse IDE
- Based on JaCoCo code coverage library
- JaCoCo is a free code coverage library for Java which has been created by the EclEmma team

### EclEmma - Counters
EclEmma supports different types of counters to be summarized in code coverage overview
- Bytecode instruction, branches, lines, methods, types, and cyclomatic complexity
- Should understand each counter and how it is measured

### EclEmma Coverage View
- Coverage view shows all analyzed Java elements within the common Java hierarchy.
- Individual columns contain the numbers for the active session, always summarizing the child elements of the respective Java element
![[Pasted image 20230830022128.png]]

### EclEmma Coverage - Source Code Annotations
- Source lines color code
	- Green for fully covered lines
	- Yellow for partly covered lines
	- red for lines that have not been executed
- Diamond color code
	- Green for fully covered branches
	- Yellow for partyle covered branches
	- Red for no branches in the particular line
![[Pasted image 20230830022308.png]]
