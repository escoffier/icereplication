cmake_minimum_required(VERSION 2.8)

project(server)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14")
#find_package(Ice CONFIG REQUIRED)
find_package(Ice REQUIRED Ice)
message(${Ice_LIBRARIES})
add_definitions(-DICE_CPP11_MAPPING)
get_filename_component(hw_proto "ices/hello.ice" ABSOLUTE)
get_filename_component(hw_proto_path "${hw_proto}" PATH)


set(hw_proto_srcs "${CMAKE_CURRENT_SOURCE_DIR}/hello.cpp")
set(hw_proto_hdrs "${CMAKE_CURRENT_SOURCE_DIR}/hello.h")
#set(hw_grpc_srcs "${CMAKE_CURRENT_BINARY_DIR}/helloworld.grpc.pb.cc")
#set(hw_grpc_hdrs "${CMAKE_CURRENT_BINARY_DIR}/helloworld.grpc.pb.h")
add_custom_command(
      OUTPUT "${hw_proto_srcs}" "${hw_proto_hdrs}" "${hw_grpc_srcs}" "${hw_grpc_hdrs}"
      COMMAND ${Ice_SLICE2CPP_EXECUTABLE}
      ARGS --output-dir ${CMAKE_CURRENT_SOURCE_DIR}  
        #--cpp_out "${CMAKE_CURRENT_BINARY_DIR}"
        #-I "${hw_proto_path}"
        #--plugin=protoc-gen-grpc="${_GRPC_CPP_PLUGIN_EXECUTABLE}"
        "${hw_proto}"
      DEPENDS "${hw_proto}")
include_directories(.)
add_executable(client Client.cpp hello.cpp)
target_link_libraries(client Ice++11 IceGrid++11 Glacier2++11 pthread)

add_executable(server Server.cpp hello.cpp HelloI.cpp)
target_link_libraries(server Ice++11 IceGrid++11 Glacier2++11 pthread)     
