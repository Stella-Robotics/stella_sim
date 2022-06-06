cmake_minimum_required(VERSION 3.8)
project(stella_sim)

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rosidl_default_generators REQUIRED)
find_package(std_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(geographic_msgs REQUIRED)
find_package(eigen_ros2 REQUIRED)

find_package(rclcpp REQUIRED)
find_package(nav_msgs REQUIRED)
find_package(std_srvs REQUIRED)
find_package(tf2_ros REQUIRED)
find_package(tf2_sensor_msgs REQUIRED)
find_package(rclpy REQUIRED)
find_package(tf2 REQUIRED)
find_package(image_transport REQUIRED)
find_package(cv_bridge REQUIRED)
find_package(tf2_geometry_msgs REQUIRED)
find_package(tf2_sensor_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(mavros_msgs REQUIRED)
find_package(std_srvs REQUIRED)
find_package(rosidl_typesupport_cpp REQUIRED)
find_package(rosidl_default_runtime REQUIRED)
find_package(OpenCV REQUIRED)

# Msg/Srv generation
rosidl_generate_interfaces(${PROJECT_NAME}
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

# ros humble
rosidl_get_typesupport_target(airsim_interface ${PROJECT_NAME} "rosidl_typesupport_cpp")

# third party dependency build
add_subdirectory(thirdparty)

# compiler settings for main applications
set(CMAKE_CXX_STANDARD 14)
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

set(CMAKE_CXX_FLAGS "-O3 -Wall -Wextra -Wnoexcept -Wstrict-null-sentinel -Wno-unused-variable -Wno-deprecated-copy -Wno-class-memaccess -fPIC")

# Now interface library between AirSim builded. -> Target : AirLib, MavLinkComLib

# add_library(airsim_settings_parser 
#   src/airsim_settings_parser.cpp
# )
# target_link_libraries(airsim_settings_parser 
#   AirLib
#   MavLinkComLib
# )

add_executable(airsim_node
  src/airsim_settings_parser.cpp
  src/airsim_ros_wrapper.cpp
  src/airsim_node.cpp
)
target_include_directories(airsim_node PRIVATE
  ${OpenCV_INCLUDE_DIRS}
)
target_link_libraries(airsim_node
  yaml-cpp
  AirLib
  MavLinkComLib
  "${airsim_interface}"
)
ament_target_dependencies(airsim_node 
  rclcpp
  sensor_msgs
  geometry_msgs
  nav_msgs
  image_transport
  tf2_ros
  cv_bridge
  std_srvs
  tf2_geometry_msgs
  tf2_sensor_msgs
)

# in, Humble Depreciated
# rosidl_target_interfaces(airsim_node
#    ${PROJECT_NAME} "rosidl_typesupport_cpp"
# )

install(TARGETS airsim_node
  DESTINATION lib/${PROJECT_NAME})
install(DIRECTORY launch rviz DESTINATION share/${PROJECT_NAME})

ament_package()