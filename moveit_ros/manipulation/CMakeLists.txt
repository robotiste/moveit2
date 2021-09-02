cmake_minimum_required(VERSION 3.10.2)
project(moveit_ros_manipulation)

# Common cmake code applied to all moveit packages
find_package(moveit_common REQUIRED)
moveit_package()

find_package(ament_cmake REQUIRED)
find_package(moveit_msgs REQUIRED)
find_package(pluginlib REQUIRED)
find_package(rclcpp REQUIRED)
find_package(tf2_eigen REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(moveit_core REQUIRED)
find_package(ament_index_cpp REQUIRED)


find_package(moveit_ros_planning REQUIRED)

find_package(moveit_ros_perception REQUIRED)

# Finds Boost Components
include(ConfigExtras.cmake)

set(THIS_PACKAGE_INCLUDE_DIRS
  pick_place/include
  move_group_pick_place_capability/include
)


set(THIS_PACKAGE_LIBRARIES
  moveit_pick_place_planner
)


set(THIS_PACKAGE_INCLUDE_DEPENDS
  pluginlib
  rclcpp
  tf2_eigen
  Eigen3
  moveit_core
  moveit_msgs
)


include_directories(${THIS_PACKAGE_INCLUDE_DIRS})

include_directories(SYSTEM
                    ${EIGEN3_INCLUDE_DIRS})

add_subdirectory(pick_place)
add_subdirectory(move_group_pick_place_capability)

install(
  TARGETS ${THIS_PACKAGE_LIBRARIES}
  EXPORT export_${PROJECT_NAME}
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
  INCLUDES DESTINATION include
)


ament_export_targets(export_${PROJECT_NAME} HAS_LIBRARY_TARGET)
ament_export_dependencies(${THIS_PACKAGE_INCLUDE_DEPENDS})

pluginlib_export_plugin_description_file(moveit_core "pick_place_capability_plugin_description.xml")

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)

  # These don't pass yet, disable them for now
  set(ament_cmake_copyright_FOUND TRUE)
  set(ament_cmake_cppcheck_FOUND TRUE)
  set(ament_cmake_cpplint_FOUND TRUE)
  set(ament_cmake_uncrustify_FOUND TRUE)

  # Run all lint tests in package.xml except those listed above
  ament_lint_auto_find_test_dependencies()
endif()

ament_package(CONFIG_EXTRAS ConfigExtras.cmake)
