cmake_minimum_required(VERSION 3.13)
project(aws_sdk_cpp_vendor)

find_package(ament_cmake REQUIRED)
find_package(ament_cmake_vendor_package REQUIRED)
find_package(AWSSDK QUIET)

set(SERVICE core;logs;monitoring;s3;kinesis;iot;lex;polly)
string(REPLACE ";" "$<SEMICOLON>" SERVICE "${SERVICE}")

ament_vendor(${PROJECT_NAME}
  SATISFIED ${AWSSDK_FOUND}
  VCS_URL https://github.com/aws/aws-sdk-cpp.git
  VCS_VERSION 1.11.169
  CMAKE_ARGS
    -DENABLE_UNITY_BUILD:BOOL=OFF
    -DENABLE_TESTING:BOOL=OFF
    -DBUILD_ONLY:STRING=${SERVICE}
    # CMake >= 4.0 removed compatibility with cmake_minimum_required < 3.5,
    # which some bundled aws-crt-cpp submodules still declare.
    -DCMAKE_POLICY_VERSION_MINIMUM=3.5
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
