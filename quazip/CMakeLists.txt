# set all include directories for in and out of source builds
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
    ${ZLIB_INCLUDE_DIR}
)

file(GLOB SRCS "*.c" "*.cpp")
file(GLOB PUBLIC_HEADERS "*.h")

set(SRCS ${SRCS} ${MOC_SRCS})
add_library(quazip STATIC ${SRCS})

# Must be added to enable export macro
target_compile_definitions(quazip PRIVATE QUAZIP_BUILD)

set_target_properties(quazip PROPERTIES VERSION 1.0.0 SOVERSION 1)
target_link_libraries(quazip Qt5::Core ${ZLIB_LIBRARY})

install(FILES ${PUBLIC_HEADERS} DESTINATION include/quazip)
install(TARGETS quazip LIBRARY DESTINATION ${LIB_DESTINATION} ARCHIVE DESTINATION ${LIB_DESTINATION} RUNTIME DESTINATION ${LIB_DESTINATION})
