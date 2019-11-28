cmake_minimum_required(VERSION 3.8.2)

project(FastNoise VERSION 1.0
  DESCRIPTION "Fast Noise CMake Project"
  LANGUAGES CXX)

option(BUILD_SHARED_LIBS "Build FastNoise as shared library" OFF)
option(FAST_NOISE_USE_DOUBLES "Use double type instead of float" ON)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_PLATFORM_INDEPENDENT_CODE ON)

set(fast_noise_target libfastnoise)

list(APPEND fast_noise_include ${CMAKE_CURRENT_SOURCE_DIR})

list(APPEND fast_noise_source FastNoise.cpp)
list(APPEND fast_noise_public_header FastNoise.h)

add_library(${fast_noise_target} ${fast_noise_source})

if (FAST_NOISE_USE_DOUBLES)
  target_compile_definitions(${fast_noise_target} PUBLIC FN_USE_DOUBLES)
endif ()

if (MSVC)
# target_compile_options(${fast_noise_target} PRIVATE /wd4273)
# target_compile_definitions(${fast_noise_target} PUBLIC HAVE_CONFIG_H)
# target_compile_definitions(${fast_noise_target} PUBLIC _CRT_SECURE_NO_DEPRECATE=1)
# target_compile_definitions(${fast_noise_target} PUBLIC _CRT_NONSTDC_NO_DEPRECATE=1)
# if(BUILD_SHARED_LIBS)
#   list(APPEND fast_noise_libraries ws2_32)
#   target_compile_definitions(${fast_noise_target} PUBLIC DLLBUILD)
# endif()
endif()

target_include_directories(${fast_noise_target} PUBLIC ${fast_noise_include})

set_target_properties(${fast_noise_target} PROPERTIES PUBLIC_HEADER
  "${fast_noise_public_header}")

if (fast_noise_libraries)
  target_link_libraries(${fast_noise_target} ${fast_noise_libraries})
endif ()

install(TARGETS ${fast_noise_target}
  LIBRARY DESTINATION lib
  PUBLIC_HEADER DESTINATION include
  ARCHIVE DESTINATION lib)

message(STATUS "")
message(STATUS "Fast Noise ${PROJECT_VERSION} BUILD SUMMARY")
message(STATUS "  Install prefix            : ${CMAKE_INSTALL_PREFIX}")
message(STATUS "  CMAKE_GENERATOR           : ${CMAKE_GENERATOR}")
message(STATUS "  CMAKE_SOURCE_DIR          : ${CMAKE_SOURCE_DIR}")
message(STATUS "  CMAKE_CURRENT_SOURCE_DIR  : ${CMAKE_CURRENT_SOURCE_DIR}")
message(STATUS "  BUILD_SHARED_LIBS         : ${BUILD_SHARED_LIBS}")
message(STATUS "  Compiler ID               : ${CMAKE_C_COMPILER_ID}")
message(STATUS "  Compiler Version          : ${CMAKE_C_COMPILER_VERSION}")
message(STATUS "  Compiler flags            : ${CMAKE_C_FLAGS}")
if(MSVC_VERSION)
message(STATUS "  MSVC Version              : ${MSVC_VERSION}")
endif(MSVC_VERSION)
