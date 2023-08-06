cmake_minimum_required(VERSION 3.13)
project(aws_sdk_cpp_vendor)

find_package(ament_cmake REQUIRED)
find_package(ament_cmake_vendor_package REQUIRED)
find_package(AWSSDK QUIET)

ament_vendor(${PROJECT_NAME}
  SATISFIED ${AWSSDK_FOUND}
  VCS_URL https://github.com/aws/aws-sdk-cpp.git
  VCS_VERSION 1.11.135
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()