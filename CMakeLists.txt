cmake_minimum_required(VERSION 3.10.0)

project(ldlidar)

set(CMAKE_CXX_STANDARD            11)
set(CMAKE_CXX_STANDARD_REQUIRED   ON)
set(CMAKE_CXX_EXTENSIONS          OFF)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

option(BUILD_SAMPLES "Build with OS specific samples" OFF)

add_library(ldlidar_driver STATIC)

set(LDLIDAR_DRIVER_SOURCES_COMMON
  ${CMAKE_CURRENT_SOURCE_DIR}/src/ldlidar_driver.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/src/ldlidar_dataprocess.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/src/ldlidar_protocol.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/src/log_module.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/src/sl_transform.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/src/slbf.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/src/tofbf.cpp
)

set(LDLIDAR_DRIVER_SOURCES_WINODWS
  ${CMAKE_CURRENT_SOURCE_DIR}/src/ldlidar_driver_win.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/src/serial_interface_win.cpp
)

set(LDLIDAR_DRIVER_SOURCES_LINUX
  ${CMAKE_CURRENT_SOURCE_DIR}/src/ldlidar_driver_linux.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/src/network_socket_interface_linux.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/src/serial_interface_linux.cpp
)

target_include_directories(ldlidar_driver
  PUBLIC
    include)

target_sources(ldlidar_driver
  PRIVATE
    ${LDLIDAR_DRIVER_SOURCES_COMMON})

if(WIN32)
  target_sources(ldlidar_driver
    PRIVATE
      ${LDLIDAR_DRIVER_SOURCES_WINODWS})

  if(BUILD_SAMPLES)
    add_subdirectory(sample/windows)
  endif()
elseif(UNIX)
  target_sources(ldlidar_driver
    PRIVATE
      ${LDLIDAR_DRIVER_SOURCES_LINUX})
      
  if(BUILD_SAMPLES)
    add_subdirectory(sample/linux)
  endif()
else()
  message(FATAL_ERROR "Linux and Windows supported only now")
endif()