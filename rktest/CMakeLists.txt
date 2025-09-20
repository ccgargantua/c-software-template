add_library(rktest rktest.c)
target_compile_definitions(rktest PRIVATE RKTEST_DEFINE_MAIN=1)
set_property(TARGET rktest PROPERTY C_STANDARD 99)

if(UNIX)
    target_link_libraries(rktest PUBLIC m)
endif()