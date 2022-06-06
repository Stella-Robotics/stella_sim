cmake_minimum_required(VERSION 3.8)
project(MavLinkCom)

# add_subdirectory("MavLinkTest")

add_library(MavLinkCom STATIC 
    common_utils/FileSystem.cpp
    common_utils/ThreadUtils.cpp
    src/AdHocConnection.cpp
    src/MavLinkConnection.cpp
    src/MavLinkFtpClient.cpp
    src/MavLinkLog.cpp
    src/MavLinkMessageBase.cpp
    src/MavLinkMessages.cpp
    src/MavLinkNode.cpp
    src/MavLinkTcpServer.cpp
    src/MavLinkVehicle.cpp
    src/MavLinkVideoStream.cpp
    src/Semaphore.cpp
    src/UdpSocket.cpp
    src/impl/AdHocConnectionImpl.cpp
    src/impl/MavLinkConnectionImpl.cpp
    src/impl/MavLinkFtpClientImpl.cpp
    src/impl/MavLinkNodeImpl.cpp
    src/impl/MavLinkTcpServerImpl.cpp
    src/impl/MavLinkVehicleImpl.cpp
    src/impl/MavLinkVideoStreamImpl.cpp
    src/impl/UdpSocketImpl.cpp
    src/serial_com/SerialPort.cpp
    src/serial_com/TcpClientPort.cpp
    src/serial_com/UdpClientPort.cpp
    src/serial_com/SocketInit.cpp
    src/serial_com/wifi.cpp
    src/impl/linux/MavLinkFindSerialPorts.cpp
)

target_include_directories(MavLinkCom PRIVATE
    common_utils
)

target_include_directories(${PROJECT_NAME} PUBLIC
  "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
  "$<INSTALL_INTERFACE:include>"
)

install(TARGETS ${PROJECT_NAME} DESTINATION lib EXPORT "export_${PROJECT_NAME}")
install(DIRECTORY include/
  DESTINATION include/${PROJECT_NAME}
  FILES_MATCHING
  PATTERN "*.h"
  PATTERN "*.hpp"
  PATTERN "*.inl"
  PATTERN "*.in" EXCLUDE)