CMAKE_MINIMUM_REQUIRED(VERSION 3.10)

project(InterprocessCommunication VERSION 1.0.0)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)

set(PROJECT_INCLUDE_DIRS ${CMAKE_SOURCE_DIR}/include)

set(SOURCES
    src/main.c
    src/shared_mem.c
    src/message_queue.c
    src/fifo.c
    src/utils.c
)

add_executable(interprocess_communication ${SOURCES})

target_include_directories(interprocess_communication PUBLIC ${PROJECT_INCLUDE_DIRS})

if (CMAKE_BUILD_TYPE STREQUAL "Debug")
    # target_compile_options(interprocess_communication PRIVATE -g -O0 -Wall -Wextra -Wpedantic -fsanitize=address)
    # target_link_options   (interprocess_communication PRIVATE -g -O0 -Wall -Wextra -Wpedantic -fsanitize=address)

    target_compile_options(interprocess_communication PRIVATE -g -O0 -Wall -Wextra -Wpedantic -fsanitize=thread,undefined)
    target_link_options   (interprocess_communication PRIVATE -g -O0 -Wall -Wextra -Wpedantic -fsanitize=thread,undefined)
endif()

set_target_properties(interprocess_communication PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin
    OUTPUT_NAME interprocess_communication
    DEBUG_POSTFIX _debug
)

find_package(MbedTLS REQUIRED)
target_link_libraries(interprocess_communication PRIVATE MbedTLS::mbedx509 MbedTLS::mbedcrypto MbedTLS::mbedtls)