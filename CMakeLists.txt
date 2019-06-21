cmake_minimum_required(VERSION 2.8.3)

# Configure CCache if available
find_program(CCACHE_FOUND ccache)
if (CCACHE_FOUND)
  set_property(GLOBAL PROPERTY RULE_LAUNCH_COMPILE ccache)
  set_property(GLOBAL PROPERTY RULE_LAUNCH_LINK ccache)
endif (CCACHE_FOUND)

project(mesh_rviz_plugins)

add_definitions("-std=c++11")
if (NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Choose the type of build, options are: Debug Release RelWithDebInfo MinSizeRel." FORCE)
endif (NOT CMAKE_BUILD_TYPE)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  rviz
  std_msgs
  nav_msgs
  sensor_msgs
  pcl_ros
  pcl_msgs
  pcl_conversions
  nodelet
  cv_bridge
  message_generation
  camera_info_manager
  image_geometry
  )

## System dependencies are found with CMake's conventions
# find_package(Boost REQUIRED COMPONENTS system)
find_package(OpenCV REQUIRED COMPONENTS core highgui imgproc features2d calib3d video)
find_package(PCL 1.7 REQUIRED COMPONENTS common)
find_package(Boost 1.54 REQUIRED)
find_package(Sophus REQUIRED)

find_package(PkgConfig REQUIRED)
pkg_check_modules(EIGEN3 REQUIRED eigen3>=3.2)
pkg_check_modules(YAML_CPP REQUIRED yaml-cpp>=0.5)

############# RVIZ #######################
  pkg_check_modules(OGRE OGRE)

  ## This plugin includes Qt widgets, so we must include Qt like so:
  ## We'll use the version that rviz used so they are compatible.
  set(CMAKE_AUTOMOC ON) # This setting causes Qt's "MOC" generation to happen automatically.
  if(rviz_QT_VERSION VERSION_LESS "5")
    message(STATUS "Using Qt4 based on the rviz_QT_VERSION: ${rviz_QT_VERSION}")
    find_package(Qt4 ${rviz_QT_VERSION} EXACT REQUIRED QtCore QtGui)
    ## pull in all required include dirs, define QT_LIBRARIES, etc.
    include(${QT_USE_FILE})
  else()
    message(STATUS "Using Qt5 based on the rviz_QT_VERSION: ${rviz_QT_VERSION}")
    find_package(Qt5 ${rviz_QT_VERSION} EXACT REQUIRED Core Widgets)
    ## make target_link_libraries(${QT_LIBRARIES}) pull in all required dependencies
    set(QT_LIBRARIES Qt5::Widgets)
  endif()

  ## Prefer the Qt signals and slots to avoid defining "emit", "slots",
  ## etc because they can conflict with boost signals, so define QT_NO_KEYWORDS here.
  add_definitions(-DQT_NO_KEYWORDS)

  include_directories(${OGRE_INCLUDE_DIRS})
  link_directories(${OGRE_LIBRARY_DIRS})
###############################################################33

# Set some compiler flags.
add_definitions("-DENABLE_SSE")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} ${SSE_FLAGS} -march=native")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} ${SSE_FLAGS} -march=native")
set(CMAKE_CXX_FLAGS_RELWITHDEBINFO "${CMAKE_CXX_FLAGS_RELWITHDEBINFO} ${SSE_FLAGS} -march=native")
set(CMAKE_CXX_FLAGS_RELWITHDEBINFO "${CMAKE_CXX_FLAGS_RELWITHDEBINFO} ${SSE_FLAGS} -march=native")

catkin_package(
 INCLUDE_DIRS src ${EIGEN3_INCLUDE_DIRS}
 LIBRARIES ${PROJECT_NAME}_nodelets
 CATKIN_DEPENDS ${REQ_CATKIN_PKGS}
 DEPENDS Boost OpenCV PCL Sophus
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(./src
  ${EIGEN3_INCLUDE_DIRS}
  ${YAML_CPP_INCLUDE_DIRS}
  ${PCL_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS}
  ${Sophus_INCLUDE_DIRS}
  ${catkin_INCLUDE_DIRS})

add_definitions("-std=c++11")
add_definitions(${PCL_DEFINITIONS})

add_subdirectory(./src)
