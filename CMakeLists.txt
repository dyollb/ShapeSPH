cmake_minimum_required(VERSION 3.18)
project(ShaeSPH CXX C)
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

find_package(MKL QUIET)
if(NOT MKL_FOUND)
  message(FATAL_ERROR "FFTW not found")
endif()

find_package(OpenMP REQUIRED)

include_directories(${CMAKE_SOURCE_DIR}/Include)
include_directories(${CMAKE_SOURCE_DIR})

if("${CMAKE_CXX_COMPILER_ID}" MATCHES "Clang")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-deprecated-declarations -Wno-inconsistent-missing-override")
endif()


set(SOFT10_SRCS
  Soft10/FFTcode.cpp
  Soft10/FST_semi_memo.cpp
  Soft10/FST_semi_memo_fftw.cpp
  Soft10/OURmods.cpp
  Soft10/OURperms.cpp
  Soft10/cospmls.cpp
  Soft10/csecond.cpp
  Soft10/fft_grids.cpp
  Soft10/fft_grids_so3.cpp
  Soft10/indextables.cpp
  Soft10/makeWigner.cpp
  Soft10/naive_synthesis.cpp
  Soft10/newFCT.cpp
  Soft10/oddweights.cpp
  Soft10/permroots.cpp
  Soft10/primitive.cpp
  Soft10/primitive_FST.cpp
  Soft10/rotate_so3.cpp
  Soft10/rotate_so3_mem.cpp
  Soft10/seminaive.cpp
  Soft10/seminaive_fftw.cpp
  Soft10/so3_correlate_fftw.cpp
  Soft10/so3_correlate_sym.cpp
  Soft10/soft.cpp
  Soft10/soft_fftw.cpp
  Soft10/soft_fftw_pc.cpp
  Soft10/soft_fftw_wo.cpp
  Soft10/soft_sym.cpp
  Soft10/utils_so3.cpp
  Soft10/utils_vec_cx.cpp
  Soft10/weights.cpp
  Soft10/wignerTransforms.cpp
  Soft10/wignerTransforms_fftw.cpp
  Soft10/wignerTransforms_sym.cpp
  #Util/CmdLineParser.cpp
  Util/PlyFile.cpp
  Util/Util.cpp
)

add_library(ShapeSPH ${SOFT10_SRCS})
target_link_libraries(ShapeSPH MKL::MKL OpenMP::OpenMP_CXX)

add_executable(ShapeAlign ShapeAlign/ShapeAlign.cpp)
target_link_libraries(ShapeAlign ShapeSPH)

add_executable(ShapeDescriptor ShapeDescriptor/ShapeDescriptor.cpp)
target_link_libraries(ShapeDescriptor ShapeSPH)

add_executable(ShapeSymmetry ShapeSymmetry/ShapeSymmetry.cpp)
target_link_libraries(ShapeSymmetry ShapeSPH)
