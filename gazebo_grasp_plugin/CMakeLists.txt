cmake_minimum_required(VERSION 3.5)
project(gazebo_grasp_plugin)

# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()


# find dependencies
find_package(ament_cmake REQUIRED)
find_package(gazebo_ros)
find_package(gazebo REQUIRED)
find_package(geometry_msgs)
find_package(std_msgs)
find_package(gazebo_version_helpers)


link_directories(${gazebo_dev_LIBRARY_DIRS})


## Declare a cpp library
add_library(gazebo_grasp_fix SHARED src/GazeboGraspFix.cpp src/GazeboGraspGripper.cpp)

target_include_directories(gazebo_grasp_fix PUBLIC include)

# linking the package against dependencies 
ament_target_dependencies(gazebo_grasp_fix
  gazebo_dev
  gazebo_ros
  gazebo_version_helpers
)

#############
## Install ##
#############

install(
  DIRECTORY include/${PROJECT_NAME}/
  DESTINATION include
)


## Mark executables and/or libraries for installation
install(TARGETS gazebo_grasp_fix
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin

)

ament_export_include_directories(include)
ament_export_libraries(gazebo_grasp_fix)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # uncomment the line when a copyright and license is not present in all source files
  #set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # uncomment the line when this package is not in a git repo
  #set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()

