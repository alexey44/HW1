cmake_minimum_required(VERSION 3.10)

set(PATCH_VERSION "1" CACHE INTERNAL "Patch version")
set(PROJECT_VESRION 0.0.${PATCH_VERSION})

project(helloWorld VERSION ${PROJECT_VESRION})

find_package(Boost COMPONENTS unit_test_framework REQUIRED)

configure_file(version.h.in version.h)

add_executable(helloWorld_cli main.cpp)

add_library(helloWorld lib.cpp)
add_executable(test_version test_version.cpp)

# set_target_properties(helloWorld_cli helloWorld test_version PROPERTIES
set_target_properties(helloWorld_cli 
    helloWorld 
    test_version 
    PROPERTIES
    CXX_STANDARD 11
    CXX_STANDARD_REQUIRED ON
)

target_include_directories(helloWorld
    PRIVATE "${CMAKE_BINARY_DIR}"
)

##------
set_target_properties(test_version PROPERTIES
    COMPILE_DEFINITIONS BOOST_TEST_DYN_LINK
    INCLUDE_DIRECTORIES ${Boost_INCLUDE_DIR}
)

target_link_libraries(helloWorld_cli PRIVATE
    helloWorld
)
target_link_libraries(test_version
    ${Boost_LIBRARIES}
    helloWorld
)
##------

target_compile_options(helloWorld_cli PRIVATE
    -Wall -Wextra -pedantic -Werror
)
target_compile_options(helloWorld PRIVATE
    -Wall -Wextra -pedantic -Werror
)
target_compile_options(test_version PRIVATE
    -Wall -Wextra -pedantic -Werror
)

install(TARGETS helloWorld_cli RUNTIME DESTINATION bin)

set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

set(CPACK_PACKAGE_CONTACT example@example.com)

include(CPack)

message(${PROJECT_VERSION_PATCH})



enable_testing()

add_test(test_version test_version)
