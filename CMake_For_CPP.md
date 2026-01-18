CMake is a **build system generator** for C++ (and other languages). It doesn’t compile your code itself; instead, it **produces build files** (like Makefiles or Visual Studio projects) that *do* the compiling.

Below is a clear, practical explanation of **how CMake works in a C++ project**, step by step.

------

## 1. The Big Picture

**Source code → CMake → Build system → Compiler → Executable / Library**

Example flow:

```
C++ files → CMakeLists.txt → Makefiles / Ninja / VS → g++ / clang / MSVC → app.exe
```

------

## 2. Key Files and Concepts

### `CMakeLists.txt`

This is the **heart of a CMake project**.

It tells CMake:

- What your project is called
- Which C++ standard to use
- What source files exist
- What executables or libraries to build
- What libraries to link

Minimal example:

```cmake
cmake_minimum_required(VERSION 3.20)

project(MyApp VERSION 1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)

add_executable(my_app
    main.cpp
    foo.cpp
)
```

------

## 3. Out-of-Source Builds (Very Important)

CMake strongly encourages **out-of-source builds**.

Project structure:

```
my_project/
├── CMakeLists.txt
├── src/
│   ├── main.cpp
│   └── foo.cpp
└── build/   ← generated files go here
```

Why?

- Keeps your source tree clean
- You can have multiple builds (Debug, Release, etc.)

------

## 4. The Two-Phase Process

### Phase 1: Configure

CMake reads `CMakeLists.txt` and checks:

- Compiler availability
- OS, architecture
- Dependencies
- Build options

Command:

```bash
cmake -S . -B build
```

Output:

- Build files (Makefile, Ninja files, `.vcxproj`)
- Cache file: `CMakeCache.txt`

------

### Phase 2: Build

Uses the generated files to compile.

```bash
cmake --build build
```

This internally runs:

- `make`
- `ninja`
- or Visual Studio build tools

------

## 5. Targets (Core Concept)

Modern CMake revolves around **targets**, not global flags.

### Executable target

```cmake
add_executable(my_app main.cpp)
```

### Library target

```cmake
add_library(my_lib foo.cpp)
```

### Link targets together

```cmake
target_link_libraries(my_app PRIVATE my_lib)
```

CMake automatically:

- Propagates include paths
- Propagates compile flags
- Handles platform differences

------

## 6. Include Directories

Instead of global includes:

```cmake
include_directories(include) ❌ (old style)
```

Use:

```cmake
target_include_directories(my_app
    PRIVATE include
)
```

Visibility:

- `PRIVATE` – only this target
- `PUBLIC` – this + dependents
- `INTERFACE` – dependents only

------

## 7. Debug vs Release

CMake supports multiple build types.

### Single-config generators (Make, Ninja)

```bash
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
```

### Multi-config generators (Visual Studio)

```bash
cmake --build build --config Release
```

------

## 8. Finding and Using Libraries

Example: using **Boost**

```cmake
find_package(Boost REQUIRED)

target_link_libraries(my_app PRIVATE Boost::boost)
```

CMake handles:

- Platform-specific names
- Paths
- Transitive dependencies

------

## 9. Why Use CMake?

✔ Cross-platform
✔ Compiler-independent
✔ Scales to large projects
✔ Widely supported (CLion, VS, VS Code)
✔ Industry standard for C++

------

## 10. Mental Model (Easy to Remember)

> **CMake describes the project, not the build steps.**

You say:

- “This is an executable”
- “It depends on this library”
- “It needs these headers”

CMake figures out:

- Compiler flags
- Link order
- Platform differences

------

## 11. Minimal Real Project Example

```
project/
├── CMakeLists.txt
└── main.cpp
```

**CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.20)
project(Hello LANGUAGES CXX)

add_executable(hello main.cpp)
```

**Build**

```bash
cmake -S . -B build
cmake --build build
./build/hello
```

from chatgpt