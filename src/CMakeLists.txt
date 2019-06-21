# FLAME_WITH_RVIZ
## Here we specify which header files need to be run through "moc",
## Qt's meta-object compiler.
set(MOC_FILES textured_mesh_display.h)

add_library(mesh_rviz_plugins
  visual.h
  textured_mesh_display.cc
  textured_mesh_visual.h
  textured_mesh_visual.cc
  surface_normals_visual.h
  surface_normals_visual.cc
  ${MOC_FILES})

## Add cmake target dependencies of the executable/library
## as an example, message headers may need to be generated before nodes
# add_dependencies(mesh_rviz_plugins_node mesh_rviz_plugins_generate_messages_cpp)
add_dependencies(mesh_rviz_plugins
  pcl_msgs_generate_messages_cpp)

target_link_libraries(mesh_rviz_plugins
  ${OGRE_LIBRARIES}
  ${QT_LIBRARIES}
  ${catkin_LIBRARIES}
  ${OpenCV_LIBS})

install(TARGETS mesh_rviz_plugins
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
