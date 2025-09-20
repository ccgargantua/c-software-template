cmake_minimum_required(VERSION 3.16)
project(prog C)

set(CMAKE_C_STANDARD 11)

option(TEST "Build test flavor" OFF)

set(SOURCE src/example_compilation_unit.c)

if (TEST)
  add_executable(prog test/rktest.c ${SOURCE})
  target_link_libraries(prog PRIVATE m)
else()
  add_executable(prog src/main.c ${SOURCE})
endif()

target_include_directories(prog PUBLIC test src)
