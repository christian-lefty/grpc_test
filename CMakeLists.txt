project(greeter)
cmake_minimum_required(VERSION 2.8)

add_definitions(
  -std=c++11
)

add_custom_command(
  OUTPUT helloworld.pb.cc helloworld.grpc.pb.cc
  COMMAND protoc ARGS --proto_path=/opt/grpc/examples/protos/ --grpc_out=. --cpp_out=. --plugin=protoc-gen-grpc=`which grpc_cpp_plugin` /opt/grpc/examples/protos/helloworld.proto
)

add_library(helloworld_proto_lib
  helloworld.pb.cc
  helloworld.grpc.pb.cc
)

target_link_libraries(helloworld_proto_lib
  grpc++
  grpc
  gpr
  protobuf
)

add_executable(greeter 
  greeter.cc
)

target_link_libraries(greeter
  helloworld_proto_lib
)

