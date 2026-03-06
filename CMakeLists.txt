cmake_minimum_required(VERSION 3.18)

project(zbitmap C)

option(BUILD_TESTS "Enable building tests" ON)

set(LZBITMAP_VERSION 1.0)

add_library(zbitmap libzbitmap.c)
set_target_properties(zbitmap PROPERTIES
    PUBLIC_HEADER libzbitmap.h
)

if(MSVC)
  target_compile_options(zbitmap PRIVATE /W4 /WX)
else()
  target_compile_options(zbitmap PRIVATE -Wconversion -Wsign-conversion -Wshadow -Wformat=2 -Wstrict-prototypes -Wmissing-prototypes -Werror)
endif()

target_include_directories(zbitmap INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})

if(BUILD_SHARED_LIBS)
    target_compile_definitions(zbitmap PUBLIC LIBZBITMAP_SHARED)
    target_compile_definitions(zbitmap PRIVATE LIBZBITMAP_EXPORT)

    if(NOT WIN32)
        target_compile_options(zbitmap PRIVATE -fvisibility=hidden)
    endif()
endif()


if(NOT WIN32)
    configure_file(
        ${CMAKE_CURRENT_SOURCE_DIR}/libzbitmap.pc.in
        ${CMAKE_CURRENT_BINARY_DIR}/libzbitmap.pc
        @ONLY
    )

    install(FILES
        ${CMAKE_CURRENT_BINARY_DIR}/libzbitmap.pc
        DESTINATION lib/pkgconfig
    )
endif()

install(TARGETS zbitmap
    LIBRARY DESTINATION lib
    PUBLIC_HEADER DESTINATION include
)

if(BUILD_TESTS)
    include(CTest)

    add_subdirectory(test)
endif()