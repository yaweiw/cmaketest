cmake_minimum_required (VERSION 2.8)
project (Tutorial)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)  # put .exe and .dll files here
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)  # put .so files here (if you ever build on Linux)
set(CMAKE_MODULE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)  # put .dll and .so files for MODULE targets here
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)  # put .lib files here

# The version number.
set (Tutorial_VERSION_MAJOR 1)
set (Tutorial_VERSION_MINOR 0)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file (
   "${PROJECT_SOURCE_DIR}/inc/TutorialConfig.h.in"
   "${PROJECT_BINARY_DIR}/inc/TutorialConfig.h"
   )

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
include_directories("${PROJECT_BINARY_DIR}")
include_directories ("${PROJECT_SOURCE_DIR}/src/MathFunctions")

# should we use our own math functions?
option (USE_MYMATH
          "Use tutorial provided math implementation" ON)

# add the MathFunctions library?
#
if (USE_MYMATH)
   include_directories ("${PROJECT_SOURCE_DIR}/src/MathFunctions")
   add_subdirectory (${PROJECT_SOURCE_DIR}/src/MathFunctions)
   set (EXTRA_LIBS ${EXTRA_LIBS} MathFunctions)
endif (USE_MYMATH)

# add the executable
add_executable(Tutorial ${PROJECT_SOURCE_DIR}/src/tutorial.c)
target_link_libraries (Tutorial MathFunctions)

# add the install targets
install (TARGETS Tutorial DESTINATION ${PROJECT_BINARY_DIR}/bin)
install (FILES "${PROJECT_BINARY_DIR}/inc/TutorialConfig.h"
                  DESTINATION ${PROJECT_BINARY_DIR}/inc)