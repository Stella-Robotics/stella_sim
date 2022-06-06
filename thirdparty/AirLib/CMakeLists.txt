cmake_minimum_required(VERSION 3.8)

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
    MavlinkCom
)

target_include_directories(${PROJECT_NAME} PUBLIC
    include
)