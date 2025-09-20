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

add_library(rktest test/rktest.c)
target_include_directories(rktest PUBLIC src)
target_include_directories(rktest PUBLIC test)
target_compile_definitions(rktest PRIVATE RKTEST_DEFINE_MAIN=1)
set_property(TARGET rktest PROPERTY C_STANDARD 99)

if(UNIX)
    target_link_libraries(rktest PUBLIC m)
endif()

set(PROJECT_OBJ
        src/example_compilation_unit.c
)

if (build_tests)
    set(TEST_SRC
            test/rktest.c
            test/example_compilation_unit_tests.c
            ${PROJECT_OBJ}
    )

    add_executable(tests ${TEST_SRC})
    target_link_libraries(tests PUBLIC rktest)

    add_executable(failing_tests ${TEST_SRC})
    target_link_libraries(failing_tests PUBLIC rktest)
    target_compile_definitions(failing_tests PRIVATE RKTEST_FAILING_TESTS=1)
else()
    add_executable(program src/main.c ${PROJECT_OBJ})
endif (build_tests)

