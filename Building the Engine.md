# Building the Engine #

The [2LoC engine](https://github.com/samaursa/tlocEngine) depends on [tlocDep](https://github.com/samaursa/tlocDep) which must be compiled first. Optionally, you can also `clone` [tlocSamples](https://github.com/samaursa/tlocSamples).

Although the build process is quite flexible on where you put your files, for now we'll assume you have the following folder structure when you clone all 3 repositories:

```
.
..
tlocDep
tlocEngine
tlocSamples
```

The build process might seem complex at first. However, our configuration allows the greatest flexibility when working with different builds.

### Repository Links

[https://github.com/samaursa/tlocDep](https://github.com/samaursa/tlocDep)

[https://github.com/samaursa/tlocEngine](https://github.com/samaursa/tlocEngine)

[https://github.com/samaursa/tlocSamples](https://github.com/samaursa/tlocSamples)

## tlocDep

Assuming you are in the `root` of the repository:

```
mkdir build
cd build
cmake ../ -DDISTRIBUTION_BUILD=ON
cmake --build . --config "Debug"
```

This will create an `INSTALL` folder in the `root` of the repository which has a `build` folder. If you had made a directory called `build_2015` then you will find a `build_2015` folder in there.

The reason we went this route is to allow different build configurations to work simultaneously. For example, we can have `VS2012, VS2013` and `VS2015` builds running simultaneously making it easier to test different builds before distributing the engine.

##tlocEngine##

The engine needs to know which `tlocDep` distribution you are using and thus it needs its `INSTALL` path.

```
mkdir build
cd build
cmake ../ -DDISTRIBUTION_BUILD=ON -DTLOC_DEP_INSTALL_PATH=../../tlocDep/INSTALL/build
cmake --build . --config "Debug"
```

**NOTE:** As pointed out before, the path `../../tlocDep/INSTALL/build` assumes the folder hierarchy shown above _and_ that you named your build folder `build`.

## tlocSamples

The samples need both the engine's and the dependency `INSTALL` paths.

```
mkdir build
cd build
cmake ../ -DTLOC_DEP_INSTALL_PATH=../../tlocDep/INSTALL/build -DTLOC_ENGINE_INSTALL_PATH=../../tlocEngine/INSTALL/build
cmake --build . --config "Debug"
```

If you want to distribute your samples, then you should enable the option `-DDISTRIBUTION_BUILD=ON` when generating the project. However, this project may have issues when debugging in Visual Studio.

Instead of overriding your current project's settings (under the `build` folder), you can start a new CMake project under a different folder e.g. `build_distr`. This setting _must_ be used if using a continuous integration server such as Jenkins.
