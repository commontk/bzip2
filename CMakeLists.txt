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

