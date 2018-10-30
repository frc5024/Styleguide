# C++
C++ is our main language when writing code to be run directly on the robot. 

This guide should make sure that all code is familiar no matter what section of the codebase you are looking at.

## C++ Version
The version of C++ that we use is currently C++ 11. Try not to include features from newer versions untill we habv officially decided that we are moving up a version. On the other hand, Make sure that the code and features you are using still work in future versions of C++.  We may upgrade in the future, and we don't want the code to break. 

## Header Files
Every `.cpp` file must have a `.hpp` file to go along with it. All our file names must use the new naming for their extensions (`.cpp` and `.hpp`).

All header files must also have a header guard.

## Header Guards
Header guards must follow the following naming standard (some of our older code does not do this) `_<FILENAME>_HG_`

For example, a file called `ShootBall.hpp` would have this header guard:
```cpp
#ifndef _SHOOTBALL_HG_
#define _SHOOTBALL_HG_

...

#endif // _SHOOTBALL_HG_
```

## Including Files
All `#include` macros should be listed in alphabetical order below a doxygen comment that describes what the file does (with a space in between)

For example:
```cpp
//! Contains a class to kill all humans

// C++ libs
#include <fstream>
#include <iostream>

// Robot libs
#include <wpilib>

// Project libs
#include "../CommandBase.hpp"
#include "../RobotCFG.hpp"

```

## Namespaces
**Never** Make use of the `using` directive. Always call functions inside a namespace the "long way". For example:

```cpp
// Good
#include <wpilib>
frc::XboxController 

// Bad
#include <wpilib>
using namespace frc

XboxController
```

## Naming Functions
The name of a function must describe what it does, or returns. Every word in the function name must also be uppercase. For example:
```
ClimbScale(); // Good

DoTheThing(); // Bad. Not descriptive

getStuff(); // Bad. Not uppercase
```

## Returning Data
When returning something from a function, there should be a space above your return statement.

Void functions should also have a return statement that looks like:
```cpp
void FancyFunction(){
	// Do stuff
	...
	
	return;
}
```

## Braces
Any code block that uses braces or multi-line brackets must use the following style:
```cpp
// Good
RobotFunction(){
	WinGame();
}

//Bad
RobotFUnction()
{
	WinGame();
}
```
Occasionally, it is useful to have an entire function on a single line. When doing this, use the following style:
```cpp
RobotFUnction(){ WinGame(); }
// Make sure there are spaces to pad the inner code
```

## Defining Classes
All classes' methods must be declared in the `.hpp` file, the defined in the `.cpp` file
