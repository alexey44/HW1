cmake_minimum_required(VERSION 3.10)

set(PATCH_VERSION "1" CACHE INTERNAL "Patch version")
set(PROJECT_VESRION 0.0.${PATCH_VERSION})

project(ohw_1_helloWorld VERSION ${PROJECT_VESRION})

# configure_file(version.h.in version.h)

add_executable(ohw_1_helloWorld_cli main.cpp)
add_library(ohw_1_helloWorld lib.cpp)
# add_executable(test_version test_version.cpp)

# set_target_properties(ohw_1_helloWorld_cli ohw_1_helloWorld test_version PROPERTIES
set_target_properties(ohw_1_helloWorld_cli 
    ohw_1_helloWorld 
    # test_version 
    PROPERTIES
    CXX_STANDARD 11
    CXX_STANDARD_REQUIRED ON
)

target_link_libraries(ohw_1_helloWorld_cli PRIVATE
    helloworld
)

target_compile_options(ohw_1_helloWorld_cli PRIVATE
        -Wall -Wextra -pedantic -Werror
    )
    target_compile_options(ohw_1_helloWorld PRIVATE
        -Wall -Wextra -pedantic -Werror
    )
    # target_compile_options(test_version PRIVATE
    #     -Wall -Wextra -pedantic -Werror
    # )

install(TARGETS ohw_1_helloWorld_cli RUNTIME DESTINATION bin)

set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

set(CPACK_PACKAGE_CONTACT example@example.com)

include(CPack)
