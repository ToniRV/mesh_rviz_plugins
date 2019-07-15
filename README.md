# mesh_rviz_plugins
RVIZ plugins to display a mesh, a refactoring of plugins in: https://github.com/robustrobotics/flame_ros

## usage

Just clone in your workspace, catkin build it, source the workspace and use rviz as you would normally do.
Then add a topic of type TexturedMesh, for which you can subscribe to pcl msgs and an image for texture.
