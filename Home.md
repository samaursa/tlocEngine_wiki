# 2LoC Engine #

The 2LoC engine is a production ready engine with an emphasis on tight error checking with an **emphasis on compile time checks** whenever possible. A major portion of the design time went into making the engine programmer friendly. We avoided vague construction of objects by creating explicit types, ensured that your types are compatible (at compile time) and emphasis on being able to figure out the code (quickly) _without_ a tutorial or documentation.

We tried to use best C++ practices (although looking back, the code can be improved a _lot_ especially with C++11 and 14 features which the engine could not use at the time).

### Features ###

**Programmer Friendly**

Most other engines are editor driven. That's their main goal, the engine is designed around the editor. 2LoC foregoes that approach and puts a heavier emphasis on making the engine friendlier for programmers.

The engine makes no attempt to compete with other engines. If you are a programmer and would like to _program_ games so that you can learn more and keep your skills sharp, then this engine may be for you.

**CI Features**

The engine uses CMake and is thus CI ready. Our build machine went offline in late 2015 in favor of another, more modern, engine that is in the works (where once again, the editor will _not_ be the main focus).

**Core Features**

The engine was built when C++11 was not widely supported. Thus, all features in the engine before ver0.2 are implemented in C++03.

* Depends only on [tlocDep](https://bitbucket.org/samaursa/tlocdep/)
* No C++ Exceptions
* Mostly standard compliant STL implementation (without C++ exceptions) - with many added features and interfaces
  * Containers
  * String
  * Iterators
  * Algorithms
  * Data structures
  * High resolution timer
* Standard compliant smart pointers including some novel smart pointers
* All smart pointers and raw pointers convertible to `VirtualPtr` which helps debug pointers e.g. dangling references, destroyed SharedPtr and UniquePtr. In `Release` it is optimized to a raw pointer
* `VirtualStackObject` which allows creation of objects on the stack with pointer semantics. This helps trace hard to find bugs when an object on the stack goes out of scope, is destroyed, but is referenced by a `VirtualPtr` (remember, a `VirtualPtr` is nothing more than a raw pointer in `Release`)
* **Data driven** with a robust and easy to extend **ECS** implementation
* Built-in logger

**Rendering**

* Custom window and input implementation (to avoid SFML, SDL dependency)
* Type-safe, RAII wrappers for OpenGL
* Compile time type checking for OpenGL
    * e.g. uniforms and attribute types are checked at compile time to avoid sending the wrong data type - this removes a full class of OpenGL bugs when working with shaders
* Novel **Shader Operator** design allowing reuse of shader variables on different materials
* Many drop-in systems to make development easier:
    * Camera system
    * ArcBall camera control system
    * Bounding box rendering system
    * Extensible debug render system
    * Static and dynamic text rendering system
    * Material system
    * Mesh render system
    * Scene graph system
    * Skybox rendering system
    * Sprite animation system (called texture animator)
    * Ray picking system (without external dependencies)
* Text rendering using Freetype
* PNG and JPEG loading using header only external libraries
* Built-in sprite support
* Rendering pipeline with command buffer

**Input, Math and Phyiscs**

* Keyboard, mouse, joystick and touch support **without external dependencies**
* Fast vector, matrix, projection types
* Utility classes including pythagoras, angle, range and scale types
* Box2D driven physics engine with rigid body dynamics and events

**Prefabs**

Not to be confused with Unity prefabs. This is where the engine truly shines:

* Quickly and easily create components and systems using the prefab library
* Many built-in prefabs including
    * Camera
    * Mesh creation
    * Scene graph related components
    * Sprite
    * Skybox
    * Textures
    * Text rendering
    * Camera control
    * Math
    * Physics

**Limitations**

* Animated rigs not supported
* Underlying data structures are not thread-safe