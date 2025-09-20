cmake_minimum_required(VERSION 3.16)
project(program)
enable_language(C)
include(CheckCCompilerFlag)

option(build_tests "Build tests" OFF)

if(MSVC)
    add_compile_options(/W4 /WX)
else()
    add_compile_options(-Wall -Wextra -Wpedantic -Werror)
endif()

add_subdirectory(rktest)

set(PROJECT_OBJ
        src/example_compilation_unit.c
)

if (build_tests)
    set(TEST_SRC
            tests/example_compilation_unit_tests.c
            ${PROJECT_OBJ}
    )

    add_executable(tests ${TEST_SRC})
    target_link_libraries(tests PUBLIC rktest)
else()
    add_executable(program src/main.c ${PROJECT_OBJ})
endif (build_tests)

