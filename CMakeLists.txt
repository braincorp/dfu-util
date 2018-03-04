project(dfu-util)
cmake_minimum_required (VERSION 3.5)

# first generate ./configure for autotools
add_custom_command(
  OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/configure
  WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
  COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/autogen.sh 
)

# generate our output
add_custom_command(
  OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/.stamp
  DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/configure
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
  COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/configure --prefix=/usr/local
  COMMAND $(MAKE)
  COMMAND mkdir -p ${CMAKE_CURRENT_BINARY_DIR}/pkg
  COMMAND $(MAKE) DESTDIR=${CMAKE_CURRENT_BINARY_DIR}/pkg install
  COMMAND touch ${CMAKE_CURRENT_BINARY_DIR}/.stamp
)

# create a nice target
add_custom_target(
  dfu-util ALL
  DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/.stamp
)

add_custom_target(
  dfu-util.clean
  COMMAND rm -rf ${CMAKE_CURRENT_BINARY_DIR}
)

# install to dfu-util component
install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/pkg/ DESTINATION / USE_SOURCE_PERMISSIONS COMPONENT dfu-util)
