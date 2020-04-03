# SPDX-License-Identifier: GPL-3.0-or-later
#
# Copyright (C) 2019 CAMELab
#
# Author: Donghyun Gouk <kukdh1@camelab.org>
#

# Set version and project name
cmake_minimum_required(VERSION 3.10)
project(mcpat)

# Set output directory
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})

# Set include directories
include_directories(
  ${PROJECT_SOURCE_DIR}
  ${PROJECT_SOURCE_DIR}/cacti
)

# Platform specific settings
if (MSVC)
  add_definitions(-D_CRT_SECURE_NO_WARNINGS)
  add_compile_options(/wd4819)  # Surpress unicode warning
  add_compile_options(/wd4305)  # Surpress truncation warning
  add_compile_options(/wd4800)  # Surpress int -> boolean waring
  add_compile_options(/wd4129)  # Surpress escape warning
  add_compile_options(/wd4244)  # Surpress conversion warning
  add_compile_options(/wd4267)  # Surpress conversion warning
else ()
  set(CMAKE_CXX_FLAGS
    "-g ${CMAKE_CXX_FLAGS}")

  if (DEBUG_BUILD)
    set(CMAKE_CXX_FLAGS "-O0 ${CMAKE_CXX_FLAGS}")
  else ()
    set(CMAKE_CXX_FLAGS "-O2 ${CMAKE_CXX_FLAGS}")
  endif ()
endif ()

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Specify source files
set(SRC_MCPAT_CACTI
  cacti/arbiter.cc
  cacti/area.cc
  cacti/bank.cc
  cacti/basic_circuit.cc
  cacti/cacti_interface.cc
  cacti/component.cc
  cacti/crossbar.cc
  cacti/decoder.cc
  cacti/htree2.cc
  cacti/io.cc
  cacti/main.cc
  cacti/mat.cc
  cacti/nuca.cc
  cacti/parameter.cc
  cacti/powergating.cc
  cacti/router.cc
  cacti/subarray.cc
  cacti/technology.cc
  cacti/uca.cc
  cacti/Ucache.cc
  cacti/wire.cc
)
set(SRC_MCPAT
  array.cc
  basic_components.cc
  core.cc
  interconnect.cc
  iocontrollers.cc
  logic.cc
  mcpat.cc
  memoryctrl.cc
  noc.cc
  processor.cc
  sharedcache.cc
  XML_Parse.cc
)

# Source group for MSVC
SOURCE_GROUP("Source Files\\" FILES ${SRC_MCPAT})
SOURCE_GROUP("Source Files\\cacti" FILES ${SRC_MCPAT_CACTI})

# Define library
add_library(mcpat STATIC
  ${SRC_MCPAT}
  ${SRC_MCPAT_CACTI}
)
