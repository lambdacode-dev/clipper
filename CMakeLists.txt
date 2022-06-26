cmake_minimum_required(VERSION 3.10)

SET(BUILD_SHARED_LIBS ON CACHE BOOL
    "Build shared libraries (.dll/.so) instead of static ones (.lib/.a)")


project(clipper VERSION 6.4.2)
add_library(clipper clipper.cpp)

target_include_directories(clipper
                           INTERFACE
                           $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
                           $<INSTALL_INTERFACE:include/polyclipping>
                           )

SET(CMAKE_BUILD_TYPE "Release" CACHE STRING "Release type")

install(TARGETS clipper EXPORT clipperTargets)
install(FILES clipper.hpp DESTINATION include/polyclipping)


install(EXPORT clipperTargets
  FILE clipperTargets.cmake
  NAMESPACE clipper::
  DESTINATION lib/cmake/clipper
)

include(CMakePackageConfigHelpers)
# generate the config file that is includes the exports
configure_package_config_file(${CMAKE_CURRENT_SOURCE_DIR}/Config.cmake.in
  "${CMAKE_CURRENT_BINARY_DIR}/clipperConfig.cmake"
  INSTALL_DESTINATION "lib/cmake/clipper"
  NO_SET_AND_CHECK_MACRO
  NO_CHECK_REQUIRED_COMPONENTS_MACRO
  )

 write_basic_package_version_file(
 "${CMAKE_CURRENT_BINARY_DIR}/clipperConfigVersion.cmake"
 VERSION "${clipper_VERSION_MAJOR}.${clipper_VERSION_MINOR}.${clipper_VERSION_PATCH}"
 COMPATIBILITY AnyNewerVersion
)
install(FILES
  ${CMAKE_CURRENT_BINARY_DIR}/clipperConfig.cmake
  ${CMAKE_CURRENT_BINARY_DIR}/clipperConfigVersion.cmake
  DESTINATION lib/cmake/clipper
  )

