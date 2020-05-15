# Styleguide

It is very important that all team members keep consistent code styling between systems, classes, 
and projects. This makes the lives of all code reviewers much easier, and keeps all our code 
understandable by everyone.

## A quick note about Lib5K

Lib5K has now seen 3 revisions to the team's styleguide, so it follows parts of all 3. It will remain to be a mix of styles and naming conventions until someone goes and cleans it (if you want to do this, feel free!)

## Project structure

Robotics projects should be set up as [Gradle sub-projects](https://docs.gradle.org/current/userguide/multi_project_builds.html). Follow this general structure:
```
repository
|
+ - /robot
|      |
|      + - src
|           |
|           + - generated 
|           |       (All generated code)
|           |
|           + - main
|                 |
|                 + - java
|                 |     (All java source code)
|                 |
|                 + - proto
|                       (All protobuf files)
|
+ - /lib5k
|       (A copy of the Lib5K source)
|
+ - /generic_robot
|       (All robot-specific code that can be reused year-to-year)
|
+ - /control_loops
|       (Python scripts for generating graphs and models)
|
+ - /third_party
        (Any libraries and tools made by other people that we need a copy of)
```

## Protobuf

Every subsystem must be accompanied by a [protobuf](https://developers.google.com/protocol-buffers) file. We generally use protobuf version 2 (not 3).

In these files, make sure to add file metadata. Here is an example:
```protobuf
syntax = "proto2";

package my.package.name;

option java_package = "io.github.frc5024.<year>.proto.my.package.name";
option java_outer_classname = "<Filename>Proto";
```

### Proto Codestyle

Follow the following rules when writing a protobuf file:

 - Indent with tabs
 - Variable names are snake_case
 - Message and Enum names follow the same naming convention as standard Java (LikeThis)

### Subsystem messages

Each subsystem needs at least the following:

 - An enum describing the name of each state for the system's state machine
   - Named: `<SystemName>State`
 - A message describing the system's goal 
   - Named: `<SystemName>Goal`
   - For example, A shooter goal could contain:
     - `required double velocity_goal`
     - `optional double velocity_epsilon`
 - A message describing the system's current status
   - Named: `<SystemName>Status`
   - Must contain a `required uint32` field for the timestamp this message was built
   - Everything else should generally be marked `optional`
   - Must contain a field for the system state machine's current state
   - Must contain a field for the system's current goal
   - Should contain the status of every sensor, and output

## Java

### Codestyle

 - All variables must be written in *camelCase* 
 - Variables should not be prefixed with `m_` (this is a change from 2019-2020)
 - All methods must be written in *camelCase*
 - All class names and enums must be written in *PascalCase*
 - All constants must be written in *UPPER_CASE_SNAKE_CASE*

### Subsystems

Each subsystem needs at least 2 files.
 - One for the actual system code
 - One for configuration

The system class should:
 - Extend `io.github.frc5024.generic_robot.subsystems.BaseSubsystem`
 - Contain a status object from the protobuf file of type `<SystemName>Status.Builder`
   - This can be created by calling `<SystemName>Status.newBuilder();` in the constructor
 - Have a public constructor
 - **Not** create motor and sensor objects in the constructor
   - All motors and sensors should be passed in to the constructor
   - This makes it much easier to unit test the code
 - Have a `getSystemStatus()` method, This method should:
   - Set the current FPGA timestamp in the status proto object 
   - Build and return a copy of the status with `status.build()`
 - Have a `isGoalAchievable` method
   - Takes in a goal object of `<SystemName>Goal`
   - Returns true if the system can get to that goal directly from it's current state. If something else needs to happen first, return false
 - Have a `StateMachine<>` object **named `stateMachine`**
 - Call the following two lines in the `periodic()` method
   - `status.setState(stateMachine.getCurrentState())`
   - `stateMachine.update()`

The system config class should:
 - Contain a field for everything that can be configured about the system
 - Contain a method called `getDefault<SystemName>()` that returns an instance of the system. 
   - This is really just the same as a `getInstance()` method, but put in a seperate file
   - This allows unit tests to make their own instance, while the robot code will call the `getDefault` method to access the class