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

