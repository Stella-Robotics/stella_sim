cmake_minimum_required(VERSION 3.8)
project(stella_sim)

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rosidl_default_generators REQUIRED)
find_package(std_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(geographic_msgs REQUIRED)
find_package(eigen_ros2 REQUIRED)

# Msg/Srv generation
rosidl_generate_interfaces(${PROJECT_NAME}_interface
  "msg/GimbalAngleEulerCmd.msg"
  "msg/GimbalAngleQuatCmd.msg"
  "msg/GPSYaw.msg"
  "msg/VelCmd.msg"
  "msg/VelCmdGroup.msg"
  "msg/CarControls.msg"
  "msg/CarState.msg"
  "msg/Altimeter.msg"
  "msg/Environment.msg"
  "srv/SetGPSPosition.srv"
  "srv/Takeoff.srv"
  "srv/TakeoffGroup.srv"
  "srv/Land.srv"
  "srv/LandGroup.srv"
  "srv/Reset.srv"
  "srv/SetLocalPosition.srv" DEPENDENCIES std_msgs geometry_msgs geographic_msgs
)

# third party dependency build
add_subdirectory(thirdparty)

# compiler settings for main applications
set(CMAKE_CXX_STANDARD 14)
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

set(CMAKE_CXX_FLAGS "-O3 -Wall -Wextra -Wnoexcept -Wstrict-null-sentinel -Wno-unused-variable -Wno-deprecated-copy -Wno-class-memaccess -fPIC")

# Now interface library between AirSim builded. -> Target : AirLib, MavLinkComLib

add_library(airsim_settings_parser 
  src/airsim_settings_parser.cpp
)
target_link_libraries(airsim_settings_parser 
  AirLib
  MavLinkComLib
)


ament_package()