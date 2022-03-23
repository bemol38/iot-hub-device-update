cmake_minimum_required (VERSION 3.5)

set (target_name adu_types)

add_library (${target_name} STATIC src/adu_types.c)

add_library (aduc::${target_name} ALIAS ${target_name})

find_package (Parson REQUIRED)

#
# Turn -fPIC on, in order to use this library in another shared library.
#
set_property (TARGET ${target_name} PROPERTY POSITION_INDEPENDENT_CODE ON)

target_include_directories (${target_name} PUBLIC inc)

target_link_libraries (${target_name} PUBLIC aduc::logging aduc::c_utils Parson::parson)

if(NOT DEFINED CMAKE_INSTALL_LIBDIR)
    set(CMAKE_INSTALL_LIBDIR "lib")
endif()

set(ADUC_UTILS_H_FILES ${CMAKE_CURRENT_LIST_DIR}/inc/aduc/adu_types.h)
install(FILES ${ADUC_UTILS_H_FILES} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/aduc)
install(TARGETS ${target_name}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/aduc
)