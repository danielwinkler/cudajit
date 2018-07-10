cmake_minimum_required(VERSION 3.10)

project(jitsphase VERSION 0.1 LANGUAGES CXX CUDA)

list(APPEND CXX_SRCS main.cpp)

if ( WIN32 )
    string(APPEND CMAKE_CUDA_STANDARD_LIBRARIES " nvrtc.lib cuda.lib" )
    string(APPEND CMAKE_CXX_STANDARD_LIBRARIES " nvrtc.lib cuda.lib" )
else ()
    link_directories(${CMAKE_CUDA_IMPLICIT_LINK_DIRECTORIES})
endif ()

add_executable(${PROJECT_NAME} ${CXX_SRCS})

target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_CUDA_TOOLKIT_INCLUDE_DIRECTORIES})
target_link_libraries(${PROJECT_NAME} nvrtc cuda)

file(GLOB CU_SCRS cuda/*.cu)
add_custom_command(TARGET ${PROJECT_NAME} PRE_BUILD
    COMMAND ${CMAKE_COMMAND} -E make_directory ${CMAKE_BINARY_DIR}/cuda)
add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND "${CMAKE_COMMAND}" -E copy
    ${CU_SCRS}
    ${CMAKE_BINARY_DIR}/cuda/
    COMMENT "Copying to output directory")
