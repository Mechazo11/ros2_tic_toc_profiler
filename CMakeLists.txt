cmake_minimum_required(VERSION 3.8)
project(ros2_tictoc_profiler)

# Use C++17, required by rclcpp
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -O3")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -O3")

# Check C++17 support
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++17" COMPILER_SUPPORTS_CXX17)

if(COMPILER_SUPPORTS_CXX17)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17")
  add_definitions(-DCOMPILEDWITHC17)
  message(STATUS "Using flag -std=c++17.")
else()
  message(FATAL_ERROR "The compiler ${CMAKE_CXX_COMPILER} has no C++17 support. Please use a different C++ compiler.")
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp)
find_package(std_msgs REQUIRED)
find_package(rosidl_default_generators REQUIRED) # For ROS2 custom messages
find_package(Boost REQUIRED COMPONENTS 
  system 
  chrono
)

# Similar to https://github.com/ros2/common_interfaces/blob/fa1d0093db5df80c886093f27810d09c46f5111e/sensor_msgs/CMakeLists.txt#L67-L73
set(msg_files
  "msg/ProfilerEntry.msg" 
)

# https://github.com/NMBURobotics/ros2_python_demos/blob/main/ros2_demo_custom_msgs/CMakeLists.txt
# This line essentially generates the custom messages to be usable within both this package and any internal libraries
rosidl_generate_interfaces(${PROJECT_NAME}
  # path to each custom message is added one per line
  ${msg_files}
)

# Header file locations [C++ node]
include_directories(include) # Add .hpp, .h files from include/package_name/

set(THIS_PACKAGE_INCLUDE_DEPENDS
  rclcpp
)

# tictoc_profiler is a library within this package
# add_library(tictoc_profiler SHARED
#   src/profiler.cpp
# )

add_library(tictoc_profiler SHARED
  src/profiler.cpp
)

# Declare build dependencies [According to ChatGPT]
ament_target_dependencies(tictoc_profiler
  ${THIS_PACKAGE_INCLUDE_DEPENDS}
)

# https://robotics.stackexchange.com/questions/23171/creating-a-custom-msg-in-ros2
rosidl_get_typesupport_target(cpp_typesupport_target ${PROJECT_NAME} "rosidl_typesupport_cpp")
target_link_libraries(tictoc_profiler "${cpp_typesupport_target}")

# Link boost with tictoc_oprofiler
target_link_libraries(tictoc_profiler
  -lboost_system
  -lboost_chrono
)


# Install all the header files in package/package/include
install (DIRECTORY include/ 
        DESTINATION include
)

# TODO find out what this does
install(
  TARGETS tictoc_profiler
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
)

ament_export_libraries(tictoc_profiler)
ament_export_include_directories(include) # Tell downstream packages where to find the headers


ament_package()
