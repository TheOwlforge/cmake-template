cmake_minimum_required(VERSION 3.10)

set(PROJECT_NAME project_name)
set(CUDA_VERSION 75)

set(SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src/ CACHE PATH "Path to src folder")
set(LIBRARY_DIR ${CMAKE_CURRENT_SOURCE_DIR}/third_party/ CACHE PATH "Path to lib folder")
set(Eigen3_INCLUDE_DIR ${LIBRARY_DIR}/eigen/ CACHE PATH "Path to Eigen source folder")
set(FreeImage_INCLUDE_DIR ${LIBRARY_DIR}/FreeImage/ CACHE PATH "Path to FreeImage source folder")

set(CMAKE_CONFIGURATION_TYPES Debug Release CACHE TYPE INTERNAL FORCE)

set(CMAKE_CXX_STANDARD 17)

if(NOT DEFINED CMAKE_CUDA_ARCHITECTURES)
  set(CMAKE_CUDA_ARCHITECTURES ${CUDA_VERSION})
endif()

############## GLM ##########################
find_package(GLM REQUIRED)
include_directories(${GLM_INCLUDE_DIRS})
link_directories(${GLM_LIBRARY_DIRS})
add_definitions(${GLM_DEFINITIONS})
if(NOT GLM_FOUND)
        message(Error "GLM not found")
endif(NOT GLM_FOUND)

project(${PROJECT_NAME} LANGUAGES CXX CUDA)
add_executable(${PROJECT_NAME} ${SRC_DIR}main.cpp)

# Visual Studio properties
set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT ${PROJECT_NAME})
set_property(TARGET ${PROJECT_NAME} PROPERTY VS_DEBUGGER_WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/)

add_subdirectory("src")