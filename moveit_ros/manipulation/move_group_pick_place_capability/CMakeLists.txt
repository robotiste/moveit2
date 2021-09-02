set(MOVEIT_LIB_NAME moveit_move_group_pick_place_capability)

include_directories(src)

add_library(${MOVEIT_LIB_NAME} SHARED
  src/pick_place_action_capability.cpp
  )
set_target_properties(${MOVEIT_LIB_NAME} PROPERTIES VERSION "${${PROJECT_NAME}_VERSION}")

target_link_libraries(${MOVEIT_LIB_NAME} 
moveit_pick_place_planner
) 

ament_target_dependencies(${MOVEIT_LIB_NAME}
  moveit_core
  rclcpp
  Boost
  class_loader
  pluginlib
)

install(DIRECTORY include/ DESTINATION include)
