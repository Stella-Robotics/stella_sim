cmake_minimum_required(VERSION 3.8)

# common compile settings for third party
find_package(Threads REQUIRED)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_FLAGS "\
-Wall -Wextra \
-Wno-class-memaccess \
-Wnon-virtual-dtor -Woverloaded-virtual \
-Wno-variadic-macros -Wno-unused-function -Wno-unused \
-pthread \
-D MSGPACK_PP_VARIADICS_MSVC=0 \
-O3 \
-Wno-deprecated-copy \
-Wno-stringop-truncation \
-Wno-unused-result \
${CMAKE_CXX_FLAGS}")
#rpclib settings
add_subdirectory(rpclib-2.3.0)
add_subdirectory(MavLinkCom)
add_subdirectory(AirLib)