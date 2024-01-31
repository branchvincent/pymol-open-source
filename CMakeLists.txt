cmake_minimum_required(VERSION 3.12)

project(${TARGET_NAME})

set( CMAKE_VERBOSE_MAKEFILE on )
add_library(${TARGET_NAME} SHARED
    ${ALL_SRC}
)

if (MSVC)
    target_compile_options(${TARGET_NAME} PRIVATE "/MP")
    set_target_properties(${TARGET_NAME} PROPERTIES SUFFIX ".pyd")
endif()

target_compile_features(${TARGET_NAME} PRIVATE cxx_std_17)

set_target_properties(${TARGET_NAME} PROPERTIES PREFIX "")

target_include_directories(${TARGET_NAME} PUBLIC
    ${ALL_INC_DIR}
)

target_link_directories(${TARGET_NAME} PUBLIC
    ${ALL_LIB_DIR}
)

if (DEFINED ALL_EXT_OBJ)
    add_library(ext_obj OBJECT IMPORTED PRIVATE)

    set_target_properties(ext_obj PROPERTIES
        IMPORTED_OBJECTS ${ALL_EXT_OBJ}
    )
    target_link_libraries(${TARGET_NAME}
    ${ALL_LIB}
    $<TARGET_OBJECTS:ext_obj>
    ${ALL_EXT_LINK}
)
else()
    target_link_libraries(${TARGET_NAME}
    ${ALL_LIB}
    ${ALL_EXT_LINK}
)
endif()

target_compile_definitions(${TARGET_NAME} PUBLIC
    ${ALL_DEF}
)
