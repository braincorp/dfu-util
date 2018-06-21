project(dfu-util)
cmake_minimum_required (VERSION 3.5)

include(ExternalProject)

ExternalProject_Add(
  dfu-util
  SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/
  CONFIGURE_COMMAND ./autogen.sh COMMAND ./configure --prefix=${CMAKE_CURRENT_BINARY_DIR}/dfu-util
  BUILD_COMMAND $(MAKE) COMMAND echo 'done'
  INSTALL_DIR ${CMAKE_CURRENT_BINARY_DIR}/dfu-util
  BUILD_IN_SOURCE 1
)

add_package_content(
  dfu-util
  DIRECTORIES ${CMAKE_CURRENT_BINARY_DIR}/dfu-util/
  DESTINATION /usr/local)
add_package_build_dependencies(
  dfu-util dfu-util)
add_debian_package(dfu-util)

