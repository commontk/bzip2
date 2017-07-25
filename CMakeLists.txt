cmake_minimum_required(VERSION 3.4)

project(bzip2 C)

if(NOT DEFINED CMAKE_DEBUG_POSTFIX)
  set(CMAKE_DEBUG_POSTFIX d)
endif()
set(CMAKE_POSITION_INDEPENDENT_CODE 1)

set(LIB_SRCS
  blocksort.c
  huffman.c
  crctable.c
  randtable.c
  compress.c
  decompress.c
  bzlib.c
  )

add_library(libbz2
  ${LIB_SRCS}
  )

if(UNIX)
  target_compile_options(libbz2
    PRIVATE -Wall -Winline
    )
endif()

target_compile_definitions(libbz2
  PRIVATE _FILE_OFFSET_BITS=64
  )

# Remove lib prefix as the target is already named libbz2
if(NOT MSVC)
  set_target_properties(libbz2 PROPERTIES PREFIX "")
endif()

# Install library
if(NOT DEFINED BZIP2_INSTALL_BIN_DIR)
  set(BZIP2_INSTALL_BIN_DIR "bin")
endif()
if(NOT DEFINED BZIP2_INSTALL_LIB_DIR)
  set(BZIP2_INSTALL_LIB_DIR "lib")
endif()
install(TARGETS libbz2
  RUNTIME DESTINATION ${BZIP2_INSTALL_BIN_DIR} COMPONENT RuntimeLibraries
  LIBRARY DESTINATION ${BZIP2_INSTALL_LIB_DIR} COMPONENT RuntimeLibraries
  ARCHIVE DESTINATION ${BZIP2_INSTALL_LIB_DIR} COMPONENT Development
  )

# Install header
if(NOT BZIP2_INSTALL_INCLUDE_DIR)
  set(BZIP2_INSTALL_INCLUDE_DIR include)
endif()
install(FILES
  bzlib.h
  DESTINATION ${BZIP2_INSTALL_INCLUDE_DIR} COMPONENT Development
  )

