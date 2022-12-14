set(OCULUS_PATH "\LIB\ovr_sdk_win_32.0.0" REQUIRED CACHE STRING "C:\Users\jacop\OneDrive\Desktop\jacopo\c++\ovr_sdk_win_32.0.0")
set(SDL_PATH "\LIB\SDL2-2.24.0" REQUIRED CACHE STRING "Path of SDL2 folder. eg: C:\\SDL2-2.0.3")

project(ZED_Stereo_Passthrough)
cmake_minimum_required(VERSION 3.1)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 OLD)
	cmake_policy(SET CMP0015 OLD)
endif(COMMAND cmake_policy)

set(CMAKE_CXX_STANDARD 11)

# Mandatory
find_package(ZED 3 REQUIRED)
find_package(GLEW REQUIRED)
find_package(CUDA REQUIRED)
find_package(OpenGL 3.0 REQUIRED)

set(OCULUS_LIBRARY_DIRS ${OCULUS_PATH}/LibOVR/Lib/Windows/x64/Release/VS2017)
set(SDL_LIBRARY_DIRS ${SDL_PATH}/lib/x64)

set(OCULUS_INCLUDE_DIRS ${OCULUS_PATH}/LibOVR/Include ${OCULUS_PATH}/LibOVRKernel/Src)
set(OCULUS_LIBRARIES LibOVR)

set(SDL_INCLUDE_DIRS ${SDL_PATH}/include)
set(SDL_LIBRARIES SDL2 SDL2main)


set(SRC_FOLDER src)
file(GLOB_RECURSE SRC_FILES "${SRC_FOLDER}/*.cpp")

add_executable(${CMAKE_PROJECT_NAME} ${SRC_FILES})
target_include_directories(${CMAKE_PROJECT_NAME} PUBLIC
	${ZED_INCLUDE_DIRS}
	${GLUT_INCLUDE_DIRS}
	${CMAKE_CURRENT_SOURCE_DIR}/include
	${OCULUS_INCLUDE_DIRS}
	${SDL_INCLUDE_DIRS}
	${CUDA_INCLUDE_DIRS}
)
target_link_directories(${CMAKE_PROJECT_NAME} PUBLIC
	${ZED_LIBRARY_DIR}
	${GLUT_LIBRARY_DIRS}
	${OpenGL_LIBRARY_DIRS}
	${SDL_LIBRARY_DIRS}
	${OCULUS_LIBRARY_DIRS}
	${CUDA_LIBRARY_DIRS}
)

add_definitions(-O3)
set_property(TARGET ${CMAKE_PROJECT_NAME} PROPERTY OUTPUT_NAME "ZED Stereo Passthrough")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /MT")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /MTd")

# Add the required libraries for linking:
target_link_libraries(${CMAKE_PROJECT_NAME} PRIVATE
	${ZED_LIBRARIES}
	${OPENGL_LIBRARIES}
	${GLUT_LIBRARY}
	${GLEW_LIBRARIES}
	${SDL_LIBRARIES}
	${OCULUS_LIBRARIES}
	${CUDA_CUDA_LIBRARY}
	${CUDA_CUDART_LIBRARY}
	${CUDA_NPP_LIBRARIES_ZED}
)
