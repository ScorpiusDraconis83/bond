![The Bond logo: a stylized glue gun](https://raw.githubusercontent.com/microsoft/bond/master/doc/src/logos/bond-logo-64x64-white.png)
<hr />

# Bond project has ended

As of March 2025, the Bond open-source project has ended. See the
[README](README.md) for more details.

The project's original README is below. It contains instructions for how to
build and use Bond as of the time of the shutdown.

# Bond

Bond is an open-source, cross-platform framework for working with schematized
data. It supports cross-language serialization/deserialization and powerful
generic mechanisms for efficiently manipulating data. Bond is broadly used at
Microsoft in high scale services.

Bond is published on GitHub at [https://github.com/microsoft/bond/](https://github.com/microsoft/bond/).

## Bond open-source project ending March 2025

The Bond open-source project will be ending development on March 31, 2025.
For more information, see [the shutdown announcement
issue](https://github.com/microsoft/bond/issues/1215).

## Documentation

For details, see the User's Manuals:

* [C++](https://microsoft.github.io/bond/manual/bond_cpp.html)
* [C#](https://microsoft.github.io/bond/manual/bond_cs.html)
* [Java](https://microsoft.github.io/bond/manual/bond_java.html)
* [Python](https://microsoft.github.io/bond/manual/bond_py.html)
* [`gbc`, the Bond compiler/codegen tool](https://microsoft.github.io/bond/manual/compiler.html)
    * See also
      [the compiler library](https://hackage.haskell.org/package/bond) that
      powers `gbc`.

For a discussion about how Bond compares to similar frameworks see [Why Bond](https://microsoft.github.io/bond/why_bond.html).

## Dependencies

Bond C++ library requires some C++11 features (currently limited to those
supported by Visual C++ 2015); a C++11 compiler is required. Additionally,
to build Bond you will need CMake (3.1+),
[Haskell Stack](https://docs.haskellstack.org/en/stable/README/#how-to-install)
(1.5.1+) and Boost (1.61+).

Additionally, Bond requires RapidJSON. The Bond repository has a Git submodules for RapidJSON. It should be cloned with the `--recursive` flag:

```bash
git clone --recursive https://github.com/microsoft/bond.git
```

If you already have RapidJSON and would like to build against it, add argument `-DBOND_FIND_RAPIDJSON=TRUE` to the CMake invocation. It will use find_package(RapidJSON). If you do not provide a RapidJSON library, Bond will also install RapidJSON.

Following are specific instructions for building on various platforms.

### Linux

Bond must be built with C++11 compiler. We test with Clang (3.8) and GNU C++
(5.4). We recommend Clang as it's faster with template-heavy code like Bond.

Run the following commands to install the minimal set of packages needed to
build the core Bond library on Ubuntu 14.04:

```bash
sudo apt-get install \
    clang \
    cmake \
    zlib1g-dev \
    libboost-dev \
    libboost-thread-dev
```

Additionally, you need the [Haskell Tool
Stack](https://docs.haskellstack.org/en/stable/README/). If your distro isn't
shipping a new enough version of it, you may encounter some non-obvious build
failures, so we recommend installing the latest Stack outside of package
management:

```bash
curl -sSL https://get.haskellstack.org/ | sh
```

In the root `bond` directory run:

```bash
mkdir build
cd build
cmake ..
make
sudo make install
```

The `build` directory is just an example. Any directory can be used as the
build destination.

To build the Bond Python module and all the C++/Python tests and
examples, a few more packages are needed.

```bash
sudo apt-get install \
    autoconf \
    build-essential \
    libboost-date-time-dev \
    libboost-python-dev \
    libboost-test-dev \
    libtool \
    python2.7-dev
```

CMake needs to be re-run with different options. This can be done after
building just the core libraries: the build tree will simply be updated with
the new options.

```bash
cd build # or wherever you ran CMake before
```

Running the following command in the `build` directory will build and execute all
the tests and examples:

```bash
make --jobs 8 check
sudo make install # To install the other libraries just built
```

(The unit tests are large so you may want to run 4-8 build jobs in parallel,
assuming you have enough memory.)

### macOS

Install Xcode and then run the following command to install the required
packages using Homebrew ([http://brew.sh/](http://brew.sh/)):

```bash
brew install \
    cmake \
    haskell-stack \
    boost \
    boost-python
```

(boost-python is optional and only needed for Python support.)

Bond can be built on macOS using either standard \*nix makefiles or Xcode. In
order to generate and build from makefiles, in the root `bond` directory run:

```bash
mkdir build
cd build
cmake ..
make
sudo make install
```

Alternatively, you can generate Xcode projects by passing the `-G Xcode` option
to cmake:

```bash
cmake -G Xcode ..
```

You can build and run unit tests by building the `check` target in Xcode or by
running make in the `build` directory:

```bash
make --jobs 8 check
```

Note that if you are using Homebrew's Python, you'll need to build
boost-python from source:

```bash
brew install --build-from-source boost-python
```

and tell cmake the location of Homebrew's libpython by setting the
`PYTHON_LIBRARY` variable, e.g.:

```bash
cmake .. \
    -DPYTHON_LIBRARY=/usr/local/Cellar/python/2.7.9/Frameworks/Python.framework/Versions/2.7/lib/libpython2.7.dylib
```

### Windows

[![Build Status](https://ci.appveyor.com/api/projects/status/7xd2a54x9cwco314/branch/master?svg=true)](https://ci.appveyor.com/project/MicrosoftBond/bond/branch/master)

Install the following tools:

- Visual Studio 2017 or newer. The following components are required:
  - .NET Framework 4.6.2 targeting pack
  - C++ development tools. A working C++ compiler is needed to build gbc.
- .NET SDK ([https://dotnet.microsoft.com/en-us/download](https://dotnet.microsoft.com/en-us/download))
- CMake ([http://www.cmake.org/download/](http://www.cmake.org/download/))
- Haskell Stack ([https://docs.haskellstack.org/en/stable/install_and_upgrade/#windows](https://docs.haskellstack.org/en/stable/install_and_upgrade/#windows))

If you are building on a network behind a proxy, set the environment variable
`HTTP_PROXY`, e.g.:

```bash
set HTTP_PROXY=http://your-proxy-name:80
```

Now you are ready to build the C# version of Bond. Open the solution file
`cs\cs.sln` in Visual Studio and build as usual. The C# unit tests can
also be run from within the solution.

To build using the .NET SDK:

```bash
dotnet restore cs\cs.sln
dotnet msbuild cs\cs.sln
```

The C++ and Python versions of Bond additionally require:

- Boost 1.61+ ([http://www.boost.org/users/download/](http://www.boost.org/users/download/))
- Python 2.7 ([https://www.python.org/downloads/](https://www.python.org/downloads/))

You may need to set the environment variables `BOOST_ROOT` and `BOOST_LIBRARYDIR`
to specify where Boost and its pre-built libraries for your environment (MSVC 12 or MSVC 14) can be
found, e.g.:

```bash
set BOOST_ROOT=D:\boost_1_61_0
set BOOST_LIBRARYDIR=D:\boost_1_61_0\lib64-msvc-14.0
```

The core Bond library and most examples only require Boost headers. The
pre-built libraries are only needed for unit tests, and Python. If Boost or
Python libraries are not found on the system, then some tests and examples
will not be built.

You can also get an appropriate version of boost using the same approach as employed
by CI.  The appveyor.yml file includes an invocation of:
```
tools\ci-scripts\windows\Install-Boost.ps1 `
                        -Version $env:BOND_BOOST `
                        -VcToolSetVer $vcToolSetVer `
                        -Components $boostComponents
```
which can also be invoked manually in order to download the relevant version, e.g.
```
Install-Boost.ps1 -Version 1.61.0 -VcToolSetVer 14.0
```
This will return the location to which the files were downloaded.  It will be a temporary
location, so you should subsequently copy the directories to a more permanent location and
then configure your environment variables to point to those locations.

To generate a solution to build the Bond Core C++ and Python with Visual
Studio 2015 run the following commands from the root `bond` directory:

```bash
mkdir build
cd build
set PreferredToolArchitecture=x64
cmake -G "Visual Studio 14 2015 Win64" ..
```

Setting `PreferredToolArchitecture=x64` selects the 64-bit toolchain which
dramatically improves build speed. (The Bond unit tests are too big to build
with 32-bit tools.)

Instead of `cmake` you can also use `cmake-gui` and specify configuration
settings in the UI. This configuration step has to be performed only once. From
then on you can use the generated solution `build\bond.sln` from Visual Studio
or build from the command line using `cmake`:

```bash
cmake --build . --target
cmake --build . --target INSTALL
```

To build and execute the unit tests and examples run:

```bash
cmake --build . --target check -- /maxcpucount:8
```

Alternatively, you can build and install Bond using the [vcpkg](https://github.com/microsoft/vcpkg/) dependency manager:

```batch
git clone https://github.com/microsoft/vcpkg.git
cd vcpkg
./bootstrap-vcpkg.bat
./vcpkg integrate install
./vcpkg install bond
```

The Bond port in vcpkg is kept up to date by Microsoft team members and community contributors.
If the version is out of date, please [create an issue or pull request in the vcpkg repository](https://github.com/microsoft/vcpkg/issues/new/choose).
