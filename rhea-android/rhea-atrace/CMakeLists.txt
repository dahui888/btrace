cmake_minimum_required(VERSION 3.4.1)

project(rheaatrace)


set(TARGET rhea-trace)

set(fastlog_DIR src/main/cpp/third_party/profilo/fastlog)
set(utils_DIR src/main/cpp/utils)

include_directories(${fastlog_DIR})
include_directories(${utils_DIR})

include_directories(src/main/cpp/include)
include_directories(src/main/cpp)

add_subdirectory(${fastlog_DIR} ${CMAKE_CURRENT_BINARY_DIR}/fastlog)
add_subdirectory(${utils_DIR} ${CMAKE_CURRENT_BINARY_DIR}/utils)

aux_source_directory(src/main/cpp/features/io SRC_DIR_ATRACE_IO)
aux_source_directory(src/main/cpp/features/trace SRC_DIR_ATRACE_MAINTHREAD)
aux_source_directory(src/main/cpp/features/memory SRC_DIR_ATRACE_MEMORY)
aux_source_directory(src/main/cpp/trace SRC_DIR_ATRACE_TRACE)
aux_source_directory(src/main/cpp/hook SRC_DIR_ATRACE_HOOK)

add_library(rhea-trace
  SHARED
  ${SRC_DIR_ATRACE_HOOK}
  ${SRC_DIR_ATRACE_IO}
  ${SRC_DIR_ATRACE_MAINTHREAD}
  ${SRC_DIR_ATRACE_MEMORY}
  ${SRC_DIR_ATRACE_TRACE}
  src/main/cpp/rhea_atrace_jni.cpp
)

find_library(log-lib log)


target_include_directories(${TARGET} PUBLIC
  ${BITS_HEADER_DIR}
)

target_link_libraries(${TARGET}
  ${log-lib}
  utils
  fastlog
  ${BITS_SO_DIR}/${CMAKE_ANDROID_ARCH_ABI}/libbytehook.so
)
