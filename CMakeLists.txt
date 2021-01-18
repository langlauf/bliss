cmake_minimum_required(VERSION 3.5)

# set the project name
project(bliss)

option(USE_GMP "Use GNU Multiple Precision Arithmetic library" OFF)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

if (MSVC)
    # /permissive- for standard C++ with "and" and "or" logical operators
    add_compile_options(/permissive-)
else()
    # Warnings, optimization, no assertions
    add_compile_options(-Wall -pedantic -O3 -DNDEBUG) #-Wextra -Werror
endif()

if(USE_GMP)
  find_path(GMP_INCLUDE_DIR NAMES gmp.h)
  find_library(GMP_LIBRARIES NAMES gmp libgmp mpir REQUIRED)
  if (MSVC)
    add_compile_options(/DBLISS_USE_GMP /I${GMP_INCLUDE_DIR})
  else()
    add_compile_options(-DBLISS_USE_GMP -I${GMP_INCLUDE_DIR})
  endif()
endif(USE_GMP)

# Add the shared library
add_library(bliss SHARED defs.cc heap.cc graph.cc orbit.cc partition.cc uintseqhash.cc utils.cc)
set_property(TARGET bliss PROPERTY POSITION_INDEPENDENT_CODE 1)

# Add the static library
add_library(bliss_static STATIC defs.cc heap.cc graph.cc orbit.cc partition.cc uintseqhash.cc utils.cc)

# Add the executable
add_executable(bliss-executable bliss.cc)
target_link_libraries(bliss-executable bliss_static)
if(USE_GMP)
  target_link_libraries(bliss-executable ${GMP_LIBRARIES})
endif(USE_GMP)
set_target_properties(bliss-executable PROPERTIES OUTPUT_NAME bliss)
