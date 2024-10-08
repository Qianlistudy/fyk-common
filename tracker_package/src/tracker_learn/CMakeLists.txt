cmake_minimum_required(VERSION 3.8)
project(tracker_learn)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)

# 创建 founder_node 可执行文件
add_executable(founder_node src/founder_node.cpp)
target_include_directories(founder_node PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)
target_compile_features(founder_node PUBLIC c_std_99 cxx_std_17)  # Require C99 and C++17
ament_target_dependencies(founder_node rclcpp std_msgs)

# 创建 tracker_node 可执行文件
add_executable(tracker_node src/tracker_node.cpp)
target_include_directories(tracker_node PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)
target_compile_features(tracker_node PUBLIC c_std_99 cxx_std_17)  # Require C99 and C++17
ament_target_dependencies(tracker_node rclcpp std_msgs)

# 安装可执行文件
install(TARGETS founder_node tracker_node
  DESTINATION lib/${PROJECT_NAME})

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
