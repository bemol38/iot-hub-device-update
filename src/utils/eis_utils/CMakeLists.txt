cmake_minimum_required (VERSION 3.5)

project (eis_utils)

set(EIS_UTILS_H_FILES
    ${CMAKE_CURRENT_LIST_DIR}/inc/eis_coms.h
    ${CMAKE_CURRENT_LIST_DIR}/inc/eis_err.h
    ${CMAKE_CURRENT_LIST_DIR}/inc/eis_utils.h
    )

set(EIS_UTILS_C_FILES
    ${CMAKE_CURRENT_LIST_DIR}/src/eis_utils.c
    ${CMAKE_CURRENT_LIST_DIR}/src/eis_coms.c
    ${CMAKE_CURRENT_LIST_DIR}/src/eis_err.c)

set(EIS_UTILS_LIBRARY ${PROJECT_NAME})

add_library (${PROJECT_NAME} STATIC ${EIS_UTILS_C_FILES})
add_library (aduc::${PROJECT_NAME} ALIAS ${PROJECT_NAME})
target_include_directories (${PROJECT_NAME} PUBLIC inc)

find_package (azure_c_shared_utility REQUIRED)
find_package (Parson REQUIRED)

target_link_libraries (
    ${PROJECT_NAME}
    PUBLIC  aduc::adu_core_interface
    PRIVATE aduc::adu_types
            aduc::c_utils
            aduc::logging
            Parson::parson
            aziotsharedutil
            uhttp)

if (ADUC_BUILD_UNIT_TESTS )
    find_package (umock_c REQUIRED CONFIG)
    target_link_libraries (${PROJECT_NAME} PRIVATE umock_c)

    add_subdirectory (tests)
endif ()

if(NOT DEFINED CMAKE_INSTALL_LIBDIR)
    set(CMAKE_INSTALL_LIBDIR "lib")
endif()

install(FILES ${EIS_UTILS_H_FILES} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/aduc)
install(TARGETS ${EIS_UTILS_LIBRARY}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/aduc
)