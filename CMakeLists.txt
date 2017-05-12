project( globalmbcpp )
cmake_minimum_required(VERSION 3.1 FATAL_ERROR)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(COMMON_SOURCE_DIR "${CMAKE_SOURCE_DIR}/src"
  CACHE PATH "Force to download the code to src")
message(STATUS "Source dir set to ${COMMON_SOURCE_DIR}")

# Disable in source building
if("${PROJECT_SOURCE_DIR}" STREQUAL "${PROJECT_BINARY_DIR}")
  message(FATAL "no in source building allowed.")
endif()

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/CMake
  ${CMAKE_SOURCE_DIR}/CMake/common)

# Default build type is Debug
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Debug)
endif(NOT CMAKE_BUILD_TYPE)

if (CMAKE_BUILD_TYPE MATCHES "Debug")
  add_definitions(-DDEBUG)
  add_definitions(-D_DEBUG)
endif()

include(GitExternal)

# Version numbers
set(LAST_RELEASE 0.0.0)
set(VERSION_MAJOR "0")
set(VERSION_MINOR "1")
set(VERSION_PATCH "0")
set(VERSION_ABI 1)

include(Common)

add_custom_target("git-status"
  COMMAND ${CMAKE_SOURCE_DIR}/git-status.sh
  WORKING_DIRECTORY "${COMMON_SOURCE_DIR}"
)

add_custom_target("git-rebase"
  COMMAND ${CMAKE_SOURCE_DIR}/git-rebase.sh ${COMMON_SOURCE_DIR}
  WORKING_DIRECTORY "${COMMON_BINARY_DIR}"
)
