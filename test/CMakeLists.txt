add_executable(zbitmap_test test.c)
target_link_libraries(zbitmap_test PRIVATE zbitmap)

if(WIN32 AND BUILD_SHARED_LIBS)
    add_custom_command(TARGET zbitmap_test POST_BUILD
                       COMMAND ${CMAKE_COMMAND} -E copy $<TARGET_FILE:zbitmap> $<TARGET_FILE_DIR:zbitmap_test>/$<TARGET_FILE_NAME:zbitmap>)
endif()

add_test(NAME zbitmap_test COMMAND $<TARGET_FILE:zbitmap_test> WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})