# /*******************************************************************************
# Copyright Intel Corporation.
# This software and the related documents are Intel copyrighted materials, and your use of them
# is governed by the express license under which they were provided to you (License).
# Unless the License provides otherwise, you may not use, modify, copy, publish, distribute, disclose
# or transmit this software or the related documents without Intel's prior written permission.
# This software and the related documents are provided as is, with no express or implied warranties,
# other than those that are expressly stated in the License.
#
# *******************************************************************************/

set(CHECKERS_LIB_NAME example_c_check)
set(CHECKERS_EXE_NAME example_c_check_exe)

AUX_SOURCE_DIRECTORY(src CHECKERS_SRC)

add_library(${CHECKERS_LIB_NAME} SHARED ${CHECKERS_SRC})
target_include_directories(${CHECKERS_LIB_NAME} PRIVATE ../../include)
target_link_libraries(${CHECKERS_LIB_NAME} json-c-static sqlite3)

add_executable (${CHECKERS_EXE_NAME} ${CHECKERS_SRC})
target_include_directories(${CHECKERS_EXE_NAME} PRIVATE ../../include)
target_link_libraries(${CHECKERS_EXE_NAME} json-c-static sqlite3)

install(TARGETS ${CHECKERS_LIB_NAME} DESTINATION ${SOURCES_INSTALL_PREFIX}/checkers_c)
install(TARGETS ${CHECKERS_EXE_NAME} DESTINATION ${SOURCES_INSTALL_PREFIX}/checkers_c)

if(BUILD_TESTS)
  add_executable(test_example
      tests/test_example.cpp
    )
  target_include_directories(test_example PRIVATE ../../include)
  target_link_libraries(test_example PRIVATE
      gtest
      gtest_main
      gmock
      gmock_main
      ${CHECKERS_LIB_NAME}
    )
  include(GoogleTest)
  gtest_discover_tests(test_example)
  install(TARGETS test_example DESTINATION ${SOURCES_INSTALL_PREFIX}/tests/unittests)
  install(FILES tests/test_example.robot DESTINATION ${SOURCES_INSTALL_PREFIX}/tests/integrationtests)
endif()
