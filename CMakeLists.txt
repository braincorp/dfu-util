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

install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/dfu-util/ DESTINATION /usr/local USE_SOURCE_PERMISSIONS COMPONENT dfu-util)

