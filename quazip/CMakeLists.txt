# set all include directories for in and out of source builds
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
)

file(GLOB SRCS "*.c" "*.cpp")
file(GLOB PUBLIC_HEADERS "*.h")

set(SRCS ${SRCS} ${MOC_SRCS})
add_library(quazip STATIC ${SRCS})

# Must be added to enable export macro
target_compile_definitions(quazip PRIVATE QUAZIP_BUILD
                  PUBLIC QUAZIP_STATIC)
target_include_directories(quazip
  INTERFACE "${CMAKE_CURRENT_LIST_DIR}/.."
  PUBLIC
       "${CMAKE_CURRENT_LIST_DIR}/../../zlib"
       "${CMAKE_CURRENT_BINARY_DIR}/../../zlib"
)

set_target_properties(quazip PROPERTIES
  VERSION 1.0.0
  SOVERSION 1
  CXX_STANDARD 14)

target_link_libraries(quazip Qt5::Core zlibstatic)

if(NOT SKIP_INSTALL_ALL)
  install(FILES ${PUBLIC_HEADERS} DESTINATION include/quazip)
  install(TARGETS quazip LIBRARY DESTINATION ${LIB_DESTINATION} ARCHIVE DESTINATION ${LIB_DESTINATION} RUNTIME DESTINATION ${LIB_DESTINATION})
endif()
cotire(quazip)
