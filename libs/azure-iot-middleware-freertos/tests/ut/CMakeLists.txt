# Copyright (c) Microsoft Corporation.
# Licensed under the MIT License.

cmake_minimum_required(VERSION 3.13)

project(az_iot_middleware_freertos_ut_tests)

if(NOT UNIX AND NOT ${CMAKE_COMPILER_IS_GNUCC})
  message(FATAL_ERROR "Unit tests must be run on Linux with GCC")
endif()

include(CTest)
enable_testing()

# CMocka test functions
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/../../cmake/modules")
include(AddCMockaTest)

# Turn on the flags necessary for code coverage
add_compile_options(-fprofile-arcs -ftest-coverage -O0 -g3)
add_link_options(-fprofile-arcs -ftest-coverage)
set(MOCK_LINKER_OPTIONS "-Wl,--gc-sections")

if("${FREERTOS_DIRECTORY}" STREQUAL "")
  message(FATAL_ERROR "The Unit tests needs a FreeRTOS directory.")
endif()

include_directories(${CMAKE_CURRENT_LIST_DIR})
include_directories(${CMAKE_CURRENT_LIST_DIR}/../config_files)
include_directories(${FREERTOS_DIRECTORY}/FreeRTOS/Source/include)
include_directories(${FREERTOS_DIRECTORY}/FreeRTOS-Plus/Source/Utilities/logging)
include_directories(${FREERTOS_DIRECTORY}/FreeRTOS/Source/portable/ThirdParty/GCC/Posix)

# Set the port for MQTT
set(AZURE_IOT_MQTT_PORT ${CMAKE_CURRENT_LIST_DIR})

# Add source files and libs
add_subdirectory(../../source source)

# Add UT
add_cmocka_test(azure_iot_ut
  SOURCES
    main.c
    azure_iot_ut.c
    azure_iot_cmocka_mqtt.c
  COMPILE_OPTIONS
    ${DEFAULT_C_COMPILE_FLAGS}
  LINK_LIBRARIES
    cmocka
    az::iot_middleware::freertos
  LINK_OPTIONS ${MOCK_LINKER_OPTIONS}
  INCLUDE_DIRECTORIES
    ${CMOCKA_INCLUDE_DIR}
    ${CMAKE_CURRENT_LIST_DIR}
    ../../source # To test private functions
)

add_cmocka_test(azure_iot_hub_client_ut
  SOURCES
    main.c
    azure_iot_hub_client_ut.c
    azure_iot_cmocka_mqtt.c
  COMPILE_OPTIONS
    ${DEFAULT_C_COMPILE_FLAGS}
  LINK_LIBRARIES
    cmocka
    az::iot_middleware::freertos
  LINK_OPTIONS ${MOCK_LINKER_OPTIONS}
  INCLUDE_DIRECTORIES
    ${CMOCKA_INCLUDE_DIR}
    ${CMAKE_CURRENT_LIST_DIR}
)

add_cmocka_test(azure_iot_hub_client_properties_ut
  SOURCES
    main.c
    azure_iot_hub_client_properties_ut.c
    azure_iot_cmocka_mqtt.c
  COMPILE_OPTIONS
    ${DEFAULT_C_COMPILE_FLAGS}
  LINK_LIBRARIES
    cmocka
    az::iot_middleware::freertos
  LINK_OPTIONS ${MOCK_LINKER_OPTIONS}
  INCLUDE_DIRECTORIES
    ${CMOCKA_INCLUDE_DIR}
    ${CMAKE_CURRENT_LIST_DIR}
)

add_cmocka_test(azure_iot_json_reader_ut
  SOURCES
    main.c
    azure_iot_json_reader_ut.c
  COMPILE_OPTIONS
    ${DEFAULT_C_COMPILE_FLAGS}
  LINK_LIBRARIES
    cmocka
    az::iot_middleware::freertos
  LINK_OPTIONS ${MOCK_LINKER_OPTIONS}
  INCLUDE_DIRECTORIES
    ${CMOCKA_INCLUDE_DIR}
    ${CMAKE_CURRENT_LIST_DIR}
)

add_cmocka_test(azure_iot_json_writer_ut
  SOURCES
    main.c
    azure_iot_json_writer_ut.c
  COMPILE_OPTIONS
    ${DEFAULT_C_COMPILE_FLAGS}
  LINK_LIBRARIES
    cmocka
    az::iot_middleware::freertos
  LINK_OPTIONS ${MOCK_LINKER_OPTIONS}
  INCLUDE_DIRECTORIES
    ${CMOCKA_INCLUDE_DIR}
    ${CMAKE_CURRENT_LIST_DIR}
)

add_cmocka_test(azure_iot_provisioning_client_ut
  SOURCES
    main.c
    azure_iot_provisioning_client_ut.c
    azure_iot_cmocka_mqtt.c
  COMPILE_OPTIONS
    ${DEFAULT_C_COMPILE_FLAGS}
  LINK_LIBRARIES
    cmocka
    az::iot_middleware::freertos
  LINK_OPTIONS ${MOCK_LINKER_OPTIONS}
  INCLUDE_DIRECTORIES
    ${CMOCKA_INCLUDE_DIR}
    ${CMAKE_CURRENT_LIST_DIR}
)
