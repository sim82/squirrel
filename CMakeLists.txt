
cmake_minimum_required(VERSION 3.11)

project(squirrel C CXX)
include(GNUInstallDirs)

message(STATUS "CMAKE_INSTALL_LIBDIR: ${CMAKE_INSTALL_LIBDIR}")
set(INSTALL_TARGETS_DEFAULT_ARGS
  EXPORT SquirrelTargets
  ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}"
  LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}"
  RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}"
)


add_subdirectory(squirrel)
add_subdirectory(sqstdlib)
add_subdirectory(sq)

if(NOT WIN32 AND NOT DISABLE_DYNAMIC)
  set_target_properties(squirrel sqstdlib PROPERTIES SOVERSION 0 VERSION 0.0.0)
endif()

set(INSTALL_CONFIGDIR ${CMAKE_INSTALL_LIBDIR}/cmake/Squirrel)


install(EXPORT SquirrelTargets
    FILE SquirrelTargets.cmake
    NAMESPACE Squirrel::
    DESTINATION ${INSTALL_CONFIGDIR}
)


include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/SquirrelConfigVersion.cmake
    VERSION 0.0.0
    COMPATIBILITY AnyNewerVersion
)

configure_package_config_file(
    ${CMAKE_CURRENT_LIST_DIR}/cmake/SquirrelConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/SquirrelConfig.cmake
    INSTALL_DESTINATION ${INSTALL_CONFIGDIR}
)

## Install all the helper files
install(
    FILES
      ${CMAKE_CURRENT_BINARY_DIR}/SquirrelConfig.cmake
      ${CMAKE_CURRENT_BINARY_DIR}/SquirrelConfigVersion.cmake
    DESTINATION ${INSTALL_CONFIGDIR}
)
