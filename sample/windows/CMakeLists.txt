cmake_minimum_required(VERSION 3.10.0)

add_executable(ldlidar_windows_demo 
  windemo.cpp
)

target_link_libraries(ldlidar_windows_demo PRIVATE ldlidar_driver)
