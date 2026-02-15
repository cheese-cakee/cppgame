# cppgame

> [!WARNING]
> Work in progress. This project is intentionally early-stage and not production-ready.

A low-level C++ game sandbox based on an SDL3 + CMake setup.

## What Exists Right Now

- SDL3 initialization and shutdown flow.
- Window creation (`800x600`) with title `Low Level Game`.
- Basic event loop handling window close (`SDL_EVENT_QUIT`).
- CMake project split into root + `lowlevelgame/` subdirectory.
- SDL3 package discovery in CMake via `find_package(SDL3 ...)`.

## What Is Not Implemented Yet

- No renderer initialization/usage yet.
- No update/render game loop structure.
- No gameplay systems, entities, scenes, or assets.
- No tests or CI pipeline.

## Current File Layout

```text
cppgame/
|-- CMakeLists.txt
|-- CMakePresets.json
|-- CMakeUserPresets.json
|-- lowlevelgame/
|   |-- CMakeLists.txt
|   |-- lowlevelgame.cpp
|   |-- lowlevelgame.h
|   |-- launch.vs.json
|-- .gitignore
|-- .gitattributes
```

## Build Notes

### Prerequisites

- CMake 3.8+
- C++ compiler (C++20 configured in subproject)
- SDL3 development package

### Configure + Build (example)

```powershell
cmake -S . -B build -G Ninja -DSDL3_DIR=<SET_YOUR_SDL3_CMAKE_PATH> -DSDL3_RUNTIME_DIR=<SET_YOUR_SDL3_RUNTIME_PATH>
cmake --build build
```

### Run (example)

```powershell
.\build\lowlevelgame\lowlevelgame.exe
```

## Project Direction

This repository is being developed slowly and intentionally by hand. Expect incremental progress over time.
