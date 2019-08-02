cmake_minimum_required(VERSION 2.8.3)
project(mesh_rviz_plugins)

find_package(catkin_simple REQUIRED)
catkin_simple(ALL_DEPS_REQUIRED)

set(CMAKE_MACOSX_RPATH 0)
add_definitions(-std=c++11 -Wall)

## System dependencies are found with CMake's conventions
find_package(PkgConfig REQUIRED)
pkg_check_modules(EIGEN3 REQUIRED eigen3>=3.2)

############# RVIZ #######################
  pkg_check_modules(OGRE OGRE)

  # This setting causes Qt's "MOC" generation to happen automatically.
  set(CMAKE_AUTOMOC ON)

  ## This plugin includes Qt widgets, so we must include Qt like so:
  ## We'll use the version that rviz used so they are compatible.
  if(rviz_QT_VERSION VERSION_LESS "5")
    message(STATUS "Using Qt4 based on the rviz_QT_VERSION: ${rviz_QT_VERSION}")
    find_package(Qt4 ${rviz_QT_VERSION} EXACT REQUIRED QtCore QtGui)
    ## pull in all required include dirs, define QT_LIBRARIES, etc.
    include(${QT_USE_FILE})
    qt4_wrap_cpp(MOC_FILES
      ${INCLUDE_FILES_QT}
    )
  else()
    message(STATUS "Using Qt5 based on the rviz_QT_VERSION: ${rviz_QT_VERSION}")
    find_package(Qt5 ${rviz_QT_VERSION} EXACT REQUIRED Core Widgets)
    ## make target_link_libraries(${QT_LIBRARIES}) pull in all required dependencies
    set(QT_LIBRARIES Qt5::Widgets)
    QT5_WRAP_CPP(MOC_FILES
      ${INCLUDE_FILES_QT}
    )
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

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(include
  ${EIGEN3_INCLUDE_DIRS})

set(MOC_FILES include/mesh_rviz_plugins/textured_mesh_display.h)
cs_add_library(${PROJECT_NAME}
  include/mesh_rviz_plugins/visual.h
  include/mesh_rviz_plugins/textured_mesh_display.h
  include/mesh_rviz_plugins/textured_mesh_visual.h
  src/textured_mesh_display.cc
  src/textured_mesh_visual.cc
  include/mesh_rviz_plugins/surface_normals_visual.h
  src/surface_normals_visual.cc
  ${MOC_FILES})


## Add cmake target dependencies of the executable/library
## as an example, message headers may need to be generated before nodes
# add_dependencies(mesh_rviz_plugins_node mesh_rviz_plugins_generate_messages_cpp)
add_dependencies(${PROJECT_NAME} pcl_msgs_generate_messages_cpp)

target_link_libraries(${PROJECT_NAME}
  ${OGRE_LIBRARIES}
  ${QT_LIBRARIES})

install(
  FILES mesh_rviz_plugins.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})

install(
  DIRECTORY icons/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/icons)

cs_install()
cs_export()
