## 1. Small / Single-Executable Project (Learning, Demos, Utilities)

**Use when:**

- One executable
- Few source files
- No reuse as a library

```
my_app/
├── src/
│   ├── main.cpp
│   ├── foo.cpp
│   └── foo.h
├── CMakeLists.txt
└── README.md
```

**Key points**

- Headers can live next to source files
- Simple and fast to navigate
- Avoid over-engineering

------

## 2. Medium Application (Multiple Modules)

**Use when:**

- One executable
- Clear subsystems (e.g., networking, UI, core logic)

```
my_app/
├── include/
│   └── my_app/
│       ├── foo.h
│       └── bar.h
├── src/
│   ├── foo.cpp
│   ├── bar.cpp
│   └── main.cpp
├── tests/
│   └── foo_test.cpp
├── CMakeLists.txt
└── README.md
```

**Why this works**

- `include/` mirrors public headers
- Makes it easier to later split into a library
- Encourages clean interfaces

------

## 3. Library Project (Static or Shared)

**Use when:**

- You are producing a reusable library
- Public vs private API matters

```
my_lib/
├── include/
│   └── my_lib/
│       ├── api.h
│       └── math.h
├── src/
│   ├── api.cpp
│   └── math.cpp
├── tests/
│   └── math_test.cpp
├── CMakeLists.txt
├── LICENSE
└── README.md
```

**Best practices**

- Only **public headers** go in `include/`
- Private headers can go in `src/`
- Namespace matches folder name (`my_lib::`)

------

## 4. Application + Internal Libraries (Most Professional Projects)

**Use when:**

- One or more executables
- Shared internal components
- Scales well for teams

```
my_project/
├── apps/
│   └── my_app/
│       ├── src/
│       │   └── main.cpp
│       └── CMakeLists.txt
├── libs/
│   └── core/
│       ├── include/core/
│       │   └── core.h
│       ├── src/
│       │   └── core.cpp
│       └── CMakeLists.txt
├── tests/
├── CMakeLists.txt
└── README.md
```

**Why this is ideal**

- Clean separation of concerns
- Internal libraries are reusable
- Mirrors how large codebases (Google, LLVM) work

------

## 5. Header-Only Library

**Use when:**

- Templates, utilities, or small libraries
- No compilation step needed

```
my_header_lib/
├── include/
│   └── my_header_lib/
│       └── utils.hpp
├── tests/
├── CMakeLists.txt
└── README.md
```

**Notes**

- Use `.hpp` or `.inl`
- Keep headers lightweight and documented
- Avoid heavy includes

------

## 6. Game / Engine-Style Project

**Use when:**

- Large system with subsystems
- Often multiple executables (editor, game, tools)

```
engine/
├── engine/
│   ├── include/
│   └── src/
├── tools/
│   └── editor/
├── game/
│   ├── include/
│   └── src/
├── assets/
├── third_party/
├── CMakeLists.txt
└── README.md
```

**Special considerations**

- Assets are versioned separately
- Third-party code is isolated
- Build system is hierarchical

------

## 7. Competitive Programming / Practice

**Use when:**

- Speed matters more than structure

```
cp/
├── main.cpp
├── snippets/
└── input.txt
```

Simple, disposable, fast.

------

## Common Best Practices (Regardless of Type)

### ✅ Naming

- Match folder names with namespaces
- Use lowercase folders, consistent style

### ✅ Build System

- **CMake is the de facto standard**
- One `CMakeLists.txt` per logical unit

### ✅ Tests

```
tests/
├── unit/
├── integration/
```

Use Catch2 / GoogleTest.

### ❌ Avoid

- Mixing headers and sources randomly
- Putting everything in `src/`
- Global include paths (`-I.`)

------

## Rule of Thumb

> **Structure for how the project will grow, not how it starts.**

