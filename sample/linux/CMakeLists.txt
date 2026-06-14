cmake_minimum_required(VERSION 3.10.0)

add_executable(ldlidar_linux_demo
  demo.cpp
)

target_link_libraries(ldlidar_linux_demo PRIVATE ldlidar_driver)
