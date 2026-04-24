project(NanoLog)

set(NANOLOG_RUNTIME_DIR ${CMAKE_CURRENT_SOURCE_DIR}/runtime)

# Build NanoLog using its native Makefile (handles preprocessor code generation)
add_custom_command(
    OUTPUT ${NANOLOG_RUNTIME_DIR}/libNanoLog.a
    COMMAND $(MAKE) libNanoLog.a
    WORKING_DIRECTORY ${NANOLOG_RUNTIME_DIR}
    COMMENT "Building NanoLog static library"
)

add_custom_target(NanoLog_build DEPENDS ${NANOLOG_RUNTIME_DIR}/libNanoLog.a)

# Import the pre-built static library
add_library(NanoLog STATIC IMPORTED GLOBAL)
set_target_properties(NanoLog PROPERTIES
    IMPORTED_LOCATION ${NANOLOG_RUNTIME_DIR}/libNanoLog.a
    INTERFACE_INCLUDE_DIRECTORIES ${NANOLOG_RUNTIME_DIR}
    INTERFACE_LINK_LIBRARIES "pthread;rt"
)
add_dependencies(NanoLog NanoLog_build)
