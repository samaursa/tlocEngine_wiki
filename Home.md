# 2LoC Engine #

## **Overview** ##

The 2LoC engine is a production ready engine with tight error checking and an **emphasis on compile time checks (asserts)** whenever possible. A major portion of the design time went into making the engine programmer friendly. We avoided ambiguous/vague function parameters when creating or working with different objects by creating explicit types. This ensures that your types are compatible (at compile time). The coding practices encourage documentation via code and avoid (redundant) commenting wherever possible.

We tried to use best C++ practices (although looking back, the code can be improved a _lot_ especially with C++11 and 14 features which the engine could not use at the time).

## **Build** ##

The [2LoC engine](https://bitbucket.org/samaursa/tlocengine) depends on [tlocDep](https://bitbucket.org/samaursa/tlocdep) which must be compiled first. Optionally, you can also `clone` [tlocSamples](https://bitbucket.org/samaursa/tlocsamples).

Although the build process is quite flexible on where you put your files, for now we'll assume you have the following folder structure when you clone all 3 repositories:

```
.
..
tlocDep
tlocEngine
tlocSamples
```

The build process might seem complex at first. However, our configuration allows the greatest flexibility when working with different builds.

###Repository Links###

[https://bitbucket.org/samaursa/tlocdep](https://bitbucket.org/samaursa/tlocdep)

[https://bitbucket.org/samaursa/tlocengine](https://bitbucket.org/samaursa/tlocengine)

[https://bitbucket.org/samaursa/tlocsamples](https://bitbucket.org/samaursa/tlocsamples)

###tlocDep###

Assuming you are in the `root` of the repository:

```
mkdir build
cd build
cmake ../ -DDISTRIBUTION_BUILD=ON
cmake --build . --config "Debug"
```

This will create an `INSTALL` folder in the `root` of the repository which has a `build` folder. If you had made a directory called `build_2015` then you will find a `build_2015` folder in there.

The reason we went this route is to allow different build configurations to work simultaneously. For example, we can have `VS2012, VS2013` and `VS2015` builds running simultaneously making it easier to test different builds before distributing the engine.

###tlocEngine###

The engine needs to know which `tlocDep` distribution you are using and thus it needs its `INSTALL` path.

```
mkdir build
cd build
cmake ../ -DDISTRIBUTION_BUILD=ON -DTLOC_DEP_INSTALL_PATH=../../tlocDep/INSTALL/build
cmake --build . --config "Debug"
```

**NOTE:** As pointed out before, the path `../../tlocDep/INSTALL/build` assumes the folder hierarchy shown above _and_ that you named your build folder `build`.

###tlocSamples###

The samples need both the engine's and the dependency `INSTALL` paths.

```
mkdir build
cd build
cmake ../ -DTLOC_DEP_INSTALL_PATH=../../tlocDep/INSTALL/build -DTLOC_ENGINE_INSTALL_PATH=../../tlocEngine/INSTALL/build
cmake --build . --config "Debug"
```

If you want to distribute your samples, then you should enable the option `-DDISTRIBUTION_BUILD=ON` when generating the project. However, this project may have issues when debugging in Visual Studio.

Instead of overriding your current project's settings (under the `build` folder), you can start a new CMake project under a different folder e.g. `build_distr`. This setting _must_ be used if using a continuous integration server such as Jenkins.

## **Features** ##

###Programmer Friendly###

Most other engines are editor driven. That's their main goal, the engine is designed around the editor. 2LoC foregoes that approach and puts a heavier emphasis on making the engine friendlier for programmers.

The engine makes no attempt to compete with other engines. If you are a programmer and would like to _program_ games so that you can learn more and keep your skills sharp, then this engine may be for you.

Our smart pointer implementations (and extensions) allow easier checking for memory leaks and segfaults. If you replace your raw pointers with `VirtualPtr` (which are optimized in `release` builds to be as fast as raw pointers) you will be able to get all the benefits albeit with some cost in runtime speed.

###CI Features###

The engine uses CMake and is thus CI ready. Our build machine went offline in late 2015 in favor of another, more modern, engine that is in the works (where once again, the editor will _not_ be the main focus).

###Tests###

Each engine library is complemented with a test project. Although the coverage is not 100%, all the major components are tested regularly. Moreover, the tests can help identify issues on machines where any or all samples do not run properly.

###Core Features###

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

###Rendering###

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

###Input, Math and Phyiscs###

* Keyboard, mouse, joystick and touch support **without external dependencies**
* Fast vector, matrix, projection types
* Utility classes including pythagoras, angle, range and scale types
* Box2D driven physics engine with rigid body dynamics and events

###Prefabs###

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

###Limitations###

* Animated rigs not supported
* Underlying data structures are not thread-safe

#Samples#

Here are some of the samples distributed with the engine:

##Input Callbacks##
![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/input_callbacks.png)

##Joysticks##
![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/joysticks.gif)

##Obj Loader##
![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/obj_loader.gif)

##Skybox##
![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/skybox.png)

##Simple Sprite##
![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/simple_sprite.gif)

##Multiple Sprite##
Sprite switching on the **same material**. Animation state is controlled by the same entity.

![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/multiple_sprites_with_anim.gif)

##Multiple Texture Coords##
Animation of multiple texture coordinates (saves space on sprite sheet)

![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/multiple_texture_coords.gif)

##FPS Camera##
![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/fps_camera.gif)

##Keyframe Animation##
![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/keyframe_anim.gif)

##2D Raypicking##
![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/raypicking_2d.gif)

##3D Raypicking##
![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/raypicking_3d.gif)

##Font as Sprite Animation##
![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/font_sprite.png)

##Font as SpriteSheet##
Efficient storage of font sprites using the [Guillotine Bin Packing](https://github.com/juj/RectangleBinPack) algorithm.

![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/font_sprite_sheet.png)

##Static Text##
Faster than dynamic text.

![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/static_text.png)

##Dynamic Text##
![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/dynamic_text.png)

##Mesh Sorting##
![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/mesh_sorting.gif)

##Bloom##
![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/bloom.png)

##Shadow Maps##
![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/shadow_map.gif)

##Godrays##
![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/godrays.png)

##Normal Mapping##
![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/normal_mapping.gif)

##Parallax Mapping##
![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/parallax_mapping.gif)

##Physics Test##
![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/physics_test.png)

##Stereoscopic Rendering##
![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/stereoscopic_rendering.png)

##Texture Stream##
Blitting to a texture.

![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/texture_stream.png)

##Texture Stream##
Each texture uses a different storage format. The image types are set at compile time (using templates). The shader operator mechanism selects the correct internal format for OpenGL simply by passing the image type - again, at compile time.

![](http://bitbucket.org/samaursa/tlocengine/wiki/imgs/texture_types.png)