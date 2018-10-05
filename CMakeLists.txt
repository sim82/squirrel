include(GNUInstallDirs)


if(MSVC)
  cmake_minimum_required(VERSION 3.4)
else()
  cmake_minimum_required(VERSION 2.8)
endif()

option(DISABLE_STATIC "Avoid building/installing static libraries.")
option(LONG_OUTPUT_NAMES "Use longer names for binaries and libraries: squirrel3 (not sq).")

set(CMAKE_INSTALL_PREFIX "${CMAKE_SOURCE_DIR}" CACHE PATH "")
if (NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE "Release")
endif ()

project(squirrel C CXX)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)

if(CMAKE_COMPILER_IS_GNUCXX)
  set(SQ_FLAGS -fno-exceptions -fno-strict-aliasing -Wall -Wextra -pedantic -Wcast-qual)

  if(CMAKE_BUILD_TYPE STREQUAL "Release")
    set(SQ_FLAGS ${SQ_FLAGS} -O3)
  elseif(CMAKE_BUILD_TYPE STREQUAL "RelWithDebInfo")
    set(SQ_FLAGS ${SQ_FLAGS} -O3 -g)
  elseif(CMAKE_BUILD_TYPE STREQUAL "MinSizeRel")
    set(SQ_FLAGS ${SQ_FLAGS} -Os)
  elseif(CMAKE_BUILD_TYPE STREQUAL "Debug")
    set(SQ_FLAGS ${SQ_FLAGS} -pg -pie -gstabs -g3 -Og)
  endif()

  if(CMAKE_VERSION VERSION_GREATER 3)
    add_compile_options(${SQ_FLAGS})
  else()
    add_definitions(${SQ_FLAGS})
  endif()

  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fno-rtti -std=c++0x")
elseif(MSVC)
  set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)
  add_definitions(-D_CRT_SECURE_NO_WARNINGS)
endif()


if(NOT SQ_DISABLE_INSTALLER)
  if(NOT INSTALL_BIN_DIR)
    set(INSTALL_BIN_DIR bin)
  endif()

  if(NOT INSTALL_LIB_DIR)
    set(INSTALL_LIB_DIR lib)
  endif()
endif()


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
