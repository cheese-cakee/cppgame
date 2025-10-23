# CMakeList.txt : CMake project for lowlevelgame, include source and define
# project specific logic here.


# Prefer Config mode so CMake looks for SDL3Config.cmake first.
# If that doesn't produce the imported target, fall back to REQUIRED mode
# which will produce a helpful error and list searched locations.
find_package(SDL3 CONFIG QUIET)

if (NOT TARGET SDL3::SDL3)
  # Try again in REQUIRED mode (will fail with a helpful message if not found)
  find_package(SDL3 REQUIRED)
endif()

if (DEFINED SDL3_DIR)
  message(STATUS "SDL3_DIR = ${SDL3_DIR}")
endif()

# Add source to this project's executable.
add_executable(lowlevelgame "lowlevelgame.cpp" "lowlevelgame.h")

if (CMAKE_VERSION VERSION_GREATER 3.12)
  set_property(TARGET lowlevelgame PROPERTY CXX_STANDARD 20)
endif()

# Link the imported SDL3 target (provided by the package config)
target_link_libraries(lowlevelgame PRIVATE SDL3::SDL3)

# Copy SDL3 runtime DLL (if SDL3_RUNTIME_DIR is provided)
if (DEFINED SDL3_RUNTIME_DIR)
  if (EXISTS "${SDL3_RUNTIME_DIR}/SDL3.dll")
    add_custom_command(TARGET lowlevelgame POST_BUILD
      COMMAND ${CMAKE_COMMAND} -E copy_if_different
              "${SDL3_RUNTIME_DIR}/SDL3.dll"
              $<TARGET_FILE_DIR:lowlevelgame>
    )
  elseif (EXISTS "${SDL3_RUNTIME_DIR}/SDL3d.dll")
    add_custom_command(TARGET lowlevelgame POST_BUILD
      COMMAND ${CMAKE_COMMAND} -E copy_if_different
              "${SDL3_RUNTIME_DIR}/SDL3d.dll"
              $<TARGET_FILE_DIR:lowlevelgame>
    )
  else()
    message(WARNING "SDL3_RUNTIME_DIR set but no SDL3.dll or SDL3d.dll found in ${SDL3_RUNTIME_DIR}")
  endif()
else()
  message(STATUS "SDL3_RUNTIME_DIR not set; ensure SDL3 DLL is on PATH or next to the executable")
endif()

# Ensure Visual Studio debug session has SDL path on PATH so the debugger can launch the exe
# Use a single property assignment; include existing PATH.
set_property(TARGET lowlevelgame PROPERTY VS_DEBUGGER_ENVIRONMENT "PATH=${SDL3_RUNTIME_DIR};%PATH%")
