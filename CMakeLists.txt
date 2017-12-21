project(dfu-util)
cmake_minimum_required (VERSION 3.5)

include(ExternalProject)

ExternalProject_Add(
  dfu-util
  SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/
  CONFIGURE_COMMAND ./autogen.sh COMMAND ./configure --prefix=${CMAKE_CURRENT_BINARY_DIR}/dfu-util
  BUILD_COMMAND make COMMAND echo 'done'
  INSTALL_DIR ${CMAKE_CURRENT_BINARY_DIR}/dfu-util
  BUILD_IN_SOURCE 1
)

set(DFU_UTIL_BINARIES 
  ${CMAKE_CURRENT_BINARY_DIR}/dfu-util/bin/dfu-util 
  ${CMAKE_CURRENT_BINARY_DIR}/dfu-util/bin/dfu-prefix 
  ${CMAKE_CURRENT_BINARY_DIR}/dfu-util/bin/dfu-suffix 
)

install(FILES ${DFU_UTIL_BINARIES} DESTINATION bin/)

set(CPACK_GENERATOR "DEB")
set(CPACK_DEBIAN_PACKAGE_NAME ${PROJECT_NAME})
set(CPACK_DEBIAN_PACKAGE_PROVIDES ${PROJECT_NAME})
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "griffin@braincorp.com")
set(CPACK_PACKAGING_INSTALL_PREFIX "/usr/local")
set(CPACK_PACKAGE_VERSION_MAJOR "0")
set(CPACK_PACKAGE_VERSION_MINOR "8")
set(CPACK_PACKAGE_VERSION_PATCH "0")

SET(CPACK_PACKAGE_EXECUTABLES dfu-util)
include(CPack)
