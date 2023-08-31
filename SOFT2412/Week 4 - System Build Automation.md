## Learning Objectives:

- Software Configuration Management
	- System Building
	- Agile System Build
- Software Build Automation Tools
	- Ant
	- Maven
	- Gradle
## Software Configuration Management
### Agile Development - Increments
In agile development, software is being incrementally developed
- Software developments build software piece by piece with each piece being a usable portion of the software
- Each iteration usually lasting two to four weeks

By developing software in increments, developers can get feedback from stakeholders more quickly and use these feedback can be used in future increments

Lifecycle:
1. Initalize product,
2. Next Iteration
3. Adjust & track
4. Record & make changes
5. Approve
6. Feedback
7. Review
8. Next Iteration

### Configuration Management (CM)
Configuration management is concerned with:
- the policies, processes, and tools for managing changing software systems. 
- Tracks of changes and component versions incorporated into each system version
- Essential for team projects to control changes made by different developers
### Configuration Management Activities
- System Building: Assembling program components, data, libraries, then compiling to an executable system
- Version Management: Keeps track of multiple versions of system components and ensuring the changes made to components  from different developers do not interfere with each other
- Change Management: Keeps track of request for changes to the software from customers and developers
	- Working out cost
	- Impact of changes
	- Which changes should be implemented
- Release Management: Preparing software for external release and keeping track of system versions that have been released for customers

### Multi-version System Development
This type of develop is basically the practice of developing and maintaining multiple versions of a system simultaneously

Say a company releases a software called Bussy 1.0 and mayn users are using it. The company wants to cater to both Windows and MacOS versions so they make new versions Bussy 1.0W for windows and Bussy 1.0M for MacOS

Say the development team wants to create a new feature, "Glizzy" so they may make a new version Bussy 1.1E

After a while, the company is ready to release Bussy 2.0 but some users are still relying on Bussy 1.0 due to specific features or because they do not want to learn the new changes thus the company supports 1.0 and 2.0 with security patches for 1.0
### Version Management
The process of managing **code-lines and baselines**, keeping track of different versions of software components in the system which these components are used and ensuring changes made by different developers do not interfere with each other

**Codeline**: Sequence of versions of source code with later versions in the sequence derived from earlier versions
- System's component often have different versions
	- One component may be in its 5th version while another on its 3rd

**Baseline**: A definition of a specific system
- Specifies the component versions that are included in the system with a specification of libraries used, configuration file, etc.
	- Snapshot or a reference point in the software's development lifecycle that specifics which versions of components make up the system at that time
- Baseline 1.0 may include Version 2 of Component A, Version 5 of Component B, specific libraries, and a config file.
- Baseline 2.0 may include newer versions of these components or new ones
- Baselines may be specified using a configuration language to define what components and in what version for a particular system
	- Useful for recreating specific version of a complete system
	- Some customers may not have the compatibility for new versions so must use legacy versions
![[Pasted image 20230822103802.png]]

### Semantic Versioning (SemVer)
Sematic Versioning is a set of rules to assign and increase version numbers for software products
- The term "Semantic" means "related to meaning" so in the context of Semantic numbers, numbers with meaning to a certain version

We do Semantic version so what we can manage versions numbers meaningfully and as software becomes bigger, more components will be integrated and these semantic numbers will help understand what each new version is

So, how do version number works, the template goes like:
`MAJOR.MINOR.PATCH` and we will increment the:
1. Major version when you make incompatible API Changes
2. Minor version when you add functionality in a backwards-compatible manner
3. Patch version when you make backwards-compatible bug fixes
![[Pasted image 20230822104423.png]]
### Agile Development in Configuration Management (CM)
In agile development, components and system are changed and updated multiple times a day and due to its nature, configuration management (CM) tools are needed to keep track of each change 

In Agile development, there is typically a shared project repository which contains the most up-to-date versions of all software components and developers will need to pull or checkout components from the repo if they want to work with them

After a developer has a copy of the component(s) in their local environment, they'll make necessary changes and once they do, they'll use system building tools to assemble and run the software locally to test their changes in isolation.

Once the changes and done and the developer is satisfied, the developer will push or check in their changes into the shared project repository

![[Pasted image 20230822105316.png]]

### System Building
Process of creating a complete, executable, system by compiling and linking the system components, external libraries, config files, etc.

System building tools and version management tools must communicate as the build process involves checking out specific component versions from the repo

## Software Build Automation Tools (Ant, Maven, Gradle)
### Build Tools - Apache Ant
Java-based software build tool for automating build process
- Requires a Java platform and is best suited for building Java projects

Does not impose any coding conventions nor any heavyweight dependency management framework
- Some build tools or framework may enforce specific coding or directory structure but Ant is very flexible
- Some build tools come with built-in management features but Ant has provisions for managing dependencies
- Uses XML files to describe how the build process should be run

```{xml}
<?xml version="1.0"?>
<project name="Hello" default="compile">
    <target name="clean" description="remove intermediate files">
        <delete dir="classes"/>
    </target>
    <target name="clobber" depends="clean" description="remove all artifact files">
        <delete file="hello.jar"/>
    </target>
    <target name="compile" description="compile the Java source code to class files">
        <mkdir dir="classes"/>
        <javac srcdir="." destdir="classes"/>
    </target>
    <target name="jar" depends="compile" description="create a Jar file for the application">
        <jar destfile="hello.jar">
            <fileset dir="classes" includes="**/*.class"/>
            <manifest>
                <attribute name="Main-Class" value="HelloProgram"/>
            </manifest>
        </jar>
    </target>
</project>
```
### Apache Maven
A build automation tool primarily used for Java projects that uses XML-based description of the software being built
- Uses a file named `pom.xml` where the project's configuration, dependencies, plugins, goals, and other attributes are defined

Maven automatically downloads project dependencies from a central repository, making dependency management much simpler.
- This central repository contains a vast number of libraries and modules that various Java projects may depend on
- You can also configure this repository other than the default (e.g. your own company-private repo)

Operates on a convention over configuration principle meaning that if you follow certain directory structures and naming conventions, Maven requires minimal configuration as it know where to find your source code, test, etc.

Supported by Eclipse, IntelliJ, JBuilder, NetBeans

Maven's functionality can be extending using Plugins, so most likely if there is some build task that you need, Maven has a plugin responsible for it

Maven's project structure will look like the image below but the Maven software tool will auto-generate the directory structure and if you write `mvn package` it will:
- Compile all java files
- Run any tests
- Package the deliverable code into a jar file

The drawbacks of using Apache Maven is that XML-based files can increase complexity and that developers using the software should be aware to follow the conventions

### Gradle
A build automation tool that builds upon the concepts of Ant and Maven
- From Ant, it borrows the flexibility to design any conceivable build process
- From Maven, it inherits conventions, dependency management, and lifecycle concepts.

Like Maven, Gradle allows certain convention for build processes however you can also redefine or customize these conventions making it adaptable to unique project requirements

Gradle uses a Domain Specific Language (DSL) based on the Groovy programming language making its build scripts more concise, readable, and expressed compared to XML-based configuration files

In Gradle, tasks, which are individual steps in the build process, are organized  using a structure called a Directed Acyclic Graph. This ensures that the tasks are executed in the right order

Gradle also supports building multiple projects together which is especially useful for larger software projects that is able to be split into smaller sub-projects

A neat feature in Gradle is that it won't rebuild everything but only the parts that have changed making the build process faster. Another neat feature is that Gradle can automatically manage and fetch these dependencies for you

### Gradle - Groovy
Gradle build files are Groovy scripts and these build files are scripts that defines and configure tasks related to building, testing, and deploying software projects

Groovy is a dynamic language of the Java Virtual Machine and it can be added as a plug-in. Because Groovy is run on the Java platform, developers can write general programming tasks in the build files

### Gradle - DSL
Gradle also presents a Domain Specific Language (DSL) tailored to the task of building code
		- A DSL is a language where it is somewhere between a programming language and a scripting language


## Gradle Basics
### Gradle - Tasks
- **Task**: A single atomic piece of work for a build
	- Compiling Classes
- **Project**: A composition of several tasks 
	- Creating of a Jar file
Each task has a **name** which can be used to refer to the task within its owning project, and a fully qualified path, which is unique across all tasks in all projects

### Gradle - Task Actions
A task in Gradle in made up of a sequence of Action objects which will execute a task
- `Action.execute(T)` to execute a task

We can add actions to a task by using these function calls:
- `Task.doFirst()`
- `Task.doLast()`

Task Action Exceptions
- StopActionException to abort an execution of the action
- StopExecutionException to abort execution of the task and continue to next task

### Gradle - Build Lifecycle
There are three main phases when you run a build on Gradle:
1. Initialization
- In the first phase of the build lifecycle, Gradle determines which projects are going to be included in the build process and If you are working with a multi-project build, Gradle will figure out which projects are part of the build and set them up for you
3. Configuration
- During this phase, Gradle sets up all the Tasks defined in your build file and every task, even if it won't be executed in the current build, will still be configured during this phase 
- Gradle also organizes the tasks into a structure known as a Directed Acyclic Graph which is a graph representing the task and their dependencies. Gradle uses this structure to build tasks according to a specific order such that the build runs smoothly
1. Execution
- During this phase of the build lifecycle, Gradle runs the tasks that were specified for the current build and in order determined by the DAG

### Gradle - Task Configuration
- **Configuration Block** - A block of code where you can set up any required variables, data structures, or configurations that the task will need when executed
	- Like a setup phase for the task
	- The configuration blocks for a task will be configured during the Configuration phase of the Gradle lifecycle
- **Closure** - A block of code specified by curly braces to capture variables from its surrounding context

```
task initalizeDatabase
initalizeDatabase << {println "conntect to database"} # Task Action
initalizeDatabase {println "configuring database connection"} #Configuration Block
```

### Gradle - Tasks are Objects
Every tasks is represented internally as an object which means that each task object has its own properties and methods which allows a high degree of control and customization for each task

Before Gradle starts executing tasks, it creates an internal representation (model) of the entire build process which includes every task and its configurations, dependencies, etc. This model is then used to effectively manage the build and ensuring the tasks are run in the correct order and that all dependencies are handled

When creating a new task without specifying its type, it'll be initialized as a `DefaultTask` but there are other task types that you can initialize. `DefaultTask` is designed to seamlessly integrate with the Gradle project and comes with foundational functionality necessary for the task to operate within a Gradle build
- Setting task dependencies
- Describing the task
- etc.

### Gradle - Methods of Default Task
| Method           | Description |
| ---------------- | ----------- |
| dependsOn(Task)  |       Adds a task as a dependency of the calling task. A depended-on task will always run before the task that depends on it      |
| doFirst(closure) |         Adds a block of executable code to the beginning of a task's action. During execution phase, the action block of every relavent task is executed    |
| doLast(closure)  |            Appends behaviour to the end of an action |
| onlyIf()                 |          Expresses a predicate which determines whether a task should be executed. The value of the predicate is the value returned by the closure   |

### Gradle - Default Task's Properties
| Method       | Description |
| ------------ | ----------- |
| didWork      |    A Boolean property indicating whether the task completed successfully         |
| enabled      |       A Boolean property indicating whether the task will execute      |
| path         |      A string property containing the fully qualified path of a task       |
| logger       |        A reference to the internal Gradle logger object     |
| logging      |    The logging property gives us access to the log level       |
| temporaryDir |         Returns a File object pointing to a temporary directory belonging to this build file. It is generally available to a task needing a temporary place in to store intermediate results of any work or to stage files for processing inside the task    |
| description             |         A small piece of human-readable metadata to document the purpose of a task    |

### Gradle - Dynamic Properties
- Properties (other than built-in ones) can be assigned to a task
- A task object functions can contain other arbitrary property names and values we want to assign to it
```
task copyFiles{
	//Find files from wherever and copies them
	// We will hardcode a list of files for illustratioon
	fileManifest = ['data.csv','config.json']
}

task createArtifact(dependsOn: copyFiles){
	println "Files in Manifest: ${copyFiles.fileManifest}"
}
	
```
### Gradle Task Types - Copy
A copy task copies files from one place into another
```
task copyFiles(type : Copy){
	from 'resources'
	into 'target'
	include '**/*/xml*', '**/*.txt*'
}
```
Note that `from`, `into`, and `include` are methods inherited from Copy

### Gradle Task Types - Jar
A Jar task creates a jar file from source files which allows efficient aggregation of multiple files into a single unit for easier distribution and deployment

To create a Jar task, we need to apply the Java plugin in Gradle which will automatically introduce several tasks related to the Java build lifecycle, include the Jar task type

This Jar task will package the main source code of the project and associated resource with its basic manifest file (metadata about the content in the jar file)

- Highly customizable
```
apply plugin: 'java'
task customJar(type: Jar){
	manifest{
		attributes firstKey: 'firstValue', secondKey: 'secondValue'
	}
	archiveName = 'hello.jar'
	destinationDir = file("${buildDir}/jars") from sourceSets.main.classes
	}
}
```
### Gradle Task Types - JavaExec
A JavaExec task runs a Java class with a main() method and it tries to take the hassle away and integrate command-line Java invocations into your build

## Gradle - Plug-ins
### Gradle - Java Plug-in
A plug-in is an extension to Gradle which configures projects that extend the build script. They can add functionalities, define conventions, and provide configurations for projects
- Specializes the way Gradle works for specific tasks

The Java plug-in adds some tasks to your project which will compile and unit test your Java source code and bundle into a JAR
- compileJava - compile Java source code
- test - runs unit test
- jar - bundle compiled code into a JAR

- Convention-based; default values for many aspects of the project are pre-defined
- Can customize projects if you do not follow the convention

### Gradle - Java Plug-in (Project Structure)
- Gradle expects to find production source code under `src/main/java` and test source code under `src/test/java`
- Files under `src/main/resources` will be included in the JAR as resources
- Files under `src/test/resources` will be included in the classpath used to run tests
- All output files are created under the build directory, with the JAR file ending up in the `build/libs` directory

### Gradle - Java Plug-in (Project Build)
The Java plug-in will add a few tasks and running `gradle tasks` will list the tasks of a project.

Gradle will compile and create a JAR file containing main classes and resources by running `gradle build`
- `gradle clean` deletes the build directory, removing all built files
- `gradle assemble` compiles and jars your code but does not run unit tests
	- Other plugs can add more artefacts to this task
- `gradle check` compiles and test your code. 
	- Other plugs add more checks to this task e.g. `checkstyle` plugin runs checkstyle against your source code

### Gradle Java Plug-in Dependencies
We can reference external JAR files that the project is dependent on in our `build.gradle` file under the dependencies code block. We can reference:
- JAR files in a repository
	- Different repositories types supported in Gradle

### Gradle - Java Plug-in (Project Customization)
The Java plug0in adds many properties with default values to a project and we can customize these default values to suit the projects needs.
- Run `Gradle properties` to list properties of a project