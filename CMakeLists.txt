cmake_minimum_required(VERSION 3.10)

project(urg_c)

set(URG_LIBRARY_DIR current)
set(URG_LIBRARY_C_INCLUDE_DIR ${URG_LIBRARY_DIR}/include/c)
set(URG_LIBRARY_SRC_DIR ${URG_LIBRARY_DIR}/src)

set (SOURCE_FILES
  ${URG_LIBRARY_C_INCLUDE_DIR}/urg_connection.h
  ${URG_LIBRARY_C_INCLUDE_DIR}/urg_debug.h
  ${URG_LIBRARY_C_INCLUDE_DIR}/urg_detect_os.h
  ${URG_LIBRARY_C_INCLUDE_DIR}/urg_errno.h
  ${URG_LIBRARY_C_INCLUDE_DIR}/urg_ring_buffer.h
  ${URG_LIBRARY_C_INCLUDE_DIR}/urg_sensor.h
  ${URG_LIBRARY_C_INCLUDE_DIR}/urg_serial_utils.h
  ${URG_LIBRARY_C_INCLUDE_DIR}/urg_serial.h
  ${URG_LIBRARY_C_INCLUDE_DIR}/urg_tcpclient.h
  ${URG_LIBRARY_C_INCLUDE_DIR}/urg_utils.h

  ${URG_LIBRARY_SRC_DIR}/urg_sensor.c
  ${URG_LIBRARY_SRC_DIR}/urg_utils.c
  ${URG_LIBRARY_SRC_DIR}/urg_debug.c
  ${URG_LIBRARY_SRC_DIR}/urg_connection.c
  ${URG_LIBRARY_SRC_DIR}/urg_ring_buffer.c
  ${URG_LIBRARY_SRC_DIR}/urg_serial.c
  ${URG_LIBRARY_SRC_DIR}/urg_serial_utils.c
  ${URG_LIBRARY_SRC_DIR}/urg_tcpclient.c
)

add_library(${PROJECT_NAME} STATIC
  ${SOURCE_FILES}
)

target_include_directories(${PROJECT_NAME} PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/${URG_LIBRARY_C_INCLUDE_DIR}>
  $<INSTALL_INTERFACE:include>
)

if (WIN32)
  target_link_libraries(${PROJECT_NAME} Ws2_32.lib Setupapi.lib)
endif()

install(TARGETS ${PROJECT_NAME}
  EXPORT ${PROJECT_NAME}
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
  INCLUDES DESTINATION include
)

install(EXPORT ${PROJECT_NAME}
  FILE UrgCTargets.cmake
  NAMESPACE UrgC::
  DESTINATION lib/cmake/UrgC
)

install(FILES UrgCConfig.cmake
  DESTINATION lib/cmake/UrgC
)
