cmake_minimum_required (VERSION 3.5)

set (target_name c_utils)

set(C_UTILS_H_FILES
    ${CMAKE_CURRENT_LIST_DIR}/inc/aduc/c_utils.h
)

set(C_UTILS_C_FILES
    ${CMAKE_CURRENT_LIST_DIR}/src/bit_ops.c
    ${CMAKE_CURRENT_LIST_DIR}/src/connection_string_utils.c
    ${CMAKE_CURRENT_LIST_DIR}/src/http_url.c
    ${CMAKE_CURRENT_LIST_DIR}/src/string_c_utils.c
    )

set(C_UTILS_LIBRARY ${target_name})

add_library (${target_name} STATIC ${C_UTILS_C_FILES})
add_library (aduc::${target_name} ALIAS ${target_name})

#
# Turn -fPIC on, in order to use this library in another shared library.
#
set_property (TARGET ${target_name} PROPERTY POSITION_INDEPENDENT_CODE ON)

find_package (azure_c_shared_utility REQUIRED)

target_include_directories (${target_name} PUBLIC inc)

target_link_libraries (${target_name} PRIVATE aduc::logging aziotsharedutil)

if (ADUC_BUILD_UNIT_TESTS)
    add_subdirectory (tests)
endif ()

if(NOT DEFINED CMAKE_INSTALL_LIBDIR)
    set(CMAKE_INSTALL_LIBDIR "lib")
endif()

install(FILES ${C_UTILS_H_FILES} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/aduc)
install(TARGETS ${C_UTILS_LIBRARY}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/aduc
)