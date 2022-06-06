cmake_minimum_required(VERSION 3.8)

project(AirLib)

file(GLOB_RECURSE ${PROJECT_NAME}_sources 
  src/api/*.cpp
  src/common/common_utils/*.cpp
  src/safety/*.cpp
  src/vehicles/car/api/*.cpp
  src/vehicles/multirotor/*.cpp
  src/vehicles/multirotor/api/*.cpp
)

add_library(${PROJECT_NAME} STATIC ${${PROJECT_NAME}_sources})

target_link_libraries(${PROJECT_NAME} PUBLIC
  eigen_ros2::eigen_ros2
  rpc
)

target_include_directories(${PROJECT_NAME} PUBLIC
  include
)

# target_include_directories(${PROJECT_NAME} PUBLIC
#   "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
#   "$<INSTALL_INTERFACE:include/${PROJECT_NAME}>"
# )

# install(TARGETS ${PROJECT_NAME} DESTINATION lib EXPORT "export_${PROJECT_NAME}")
# install(DIRECTORY include/
#   DESTINATION include/${PROJECT_NAME}
#   FILES_MATCHING
#   PATTERN "*.h"
#   PATTERN "*.hpp"
#   PATTERN "*.inl"
#   PATTERN "*.in" EXCLUDE)