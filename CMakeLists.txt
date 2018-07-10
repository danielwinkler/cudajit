cmake_minimum_required(VERSION 3.10)

project(jitsphase VERSION 0.1 LANGUAGES CXX CUDA)

list(APPEND CXX_SRCS main.cpp)

if ( WIN32 )
    string(APPEND CMAKE_CUDA_STANDARD_LIBRARIES " nvrtc.lib cuda.lib" )
    string(APPEND CMAKE_CXX_STANDARD_LIBRARIES " nvrtc.lib cuda.lib" )
else ()
    message(FATAL "TODO")
endif ()

add_executable(${PROJECT_NAME} ${CXX_SRCS})