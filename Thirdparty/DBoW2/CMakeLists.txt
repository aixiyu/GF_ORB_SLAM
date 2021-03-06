cmake_minimum_required(VERSION 2.8)
project(DBoW2)

IF(NOT CMAKE_BUILD_TYPE)
  SET(CMAKE_BUILD_TYPE Release)
ENDIF()
MESSAGE(STATUS "BUILD TYPE:" ${CMAKE_BUILD_TYPE})

IF(${CMAKE_SYSTEM_PROCESSOR} MATCHES "arm") 
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -Wall -O3 -ffast-math -flto -march=armv8-a+crypto -mcpu=cortex-a57+crypto")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -O3 -ffast-math -flto -march=armv8-a+crypto -mcpu=cortex-a57+crypto")
ELSE()
 set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -Wall  -O3 -march=native ")
 set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall  -O3 -march=native")
ENDIF()

# set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -Wall  -O3 -march=native ")
# set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall  -O3 -march=native")
SET(CUDA_USE_STATIC_CUDA_RUNTIME OFF)

set(HDRS_DBOW2
  DBoW2/BowVector.h
  DBoW2/FORB.h 
  DBoW2/FClass.h       
  DBoW2/FeatureVector.h
  DBoW2/ScoringObject.h   
  DBoW2/TemplatedVocabulary.h)
set(SRCS_DBOW2
  DBoW2/BowVector.cpp
  DBoW2/FORB.cpp      
  DBoW2/FeatureVector.cpp
  DBoW2/ScoringObject.cpp)

set(HDRS_DUTILS
  DUtils/Random.h
  DUtils/Timestamp.h)
set(SRCS_DUTILS
  DUtils/Random.cpp
  DUtils/Timestamp.cpp)

if ($ENV{ROS_DISTRO} STREQUAL "indigo")
    set( OpenCV_DIR /opt/opencv2/share/OpenCV )
    find_package(OpenCV 2.4.13 REQUIRED)
elseif ($ENV{ROS_DISTRO} STREQUAL "kinetic")
    # set( OpenCV_DIR /opt/opencv-3.4.1/share/OpenCV )
    set( OpenCV_DIR /opt/opencv3/share/OpenCV )
    find_package(OpenCV 3.3.1 REQUIRED)
endif()
MESSAGE("OpenCV include dir: " ${OpenCV_INCLUDE_DIRS})

set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

include_directories(${OpenCV_INCLUDE_DIRS})
add_library(DBoW2 SHARED ${SRCS_DBOW2} ${SRCS_DUTILS})
target_link_libraries(DBoW2 ${OpenCV_LIBS})

