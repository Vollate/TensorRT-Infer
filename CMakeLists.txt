cmake_minimum_required(VERSION 3.16)
project(TrtInfer LANGUAGES CXX CUDA)

include_directories($ENV{TensorRT_ROOT}/include)
set(CUDA_TOOLKIT_ROOT_DIR $ENV{CUDA_PATH})
find_package(CUDA REQUIRED)

add_library(trtinfer src/infer.cu src/yolo.cu)

target_link_directories(trtinfer PRIVATE $ENV{TensorRT_ROOT}/lib)
target_link_libraries(trtinfer PRIVATE nvinfer)
# set your GPU arch here
set_target_properties(trtinfer PROPERTIES CUDA_ARCHITECTURES "86")

# set your install path here
set(install_destination "/home/lambillda/SDK/trtInfer")
install(
  TARGETS trtinfer
  EXPORT trt_infer
  DESTINATION "${install_destination}/lib")
install(FILES "src/infer.hpp" "src/yolo.hpp"
        DESTINATION "${install_destination}/include")
