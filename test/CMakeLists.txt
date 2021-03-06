if (BUILD_TESTS)

    include_directories(${PROJECT_SOURCE_DIR}/lib/ ${PROJECT_SOURCE_DIR}/cli/)
    include_directories(SYSTEM ${PROJECT_SOURCE_DIR}/externals/tinyxml)

    file(GLOB hdrs "*.h")
    file(GLOB srcs "*.cpp")

    add_executable(testrunner ${hdrs} ${srcs} $<TARGET_OBJECTS:lib_objs> $<TARGET_OBJECTS:cli_objs> $<TARGET_OBJECTS:tinyxml_objs>)
    if (HAVE_RULES)
        target_link_libraries(testunner pcre)
    endif()

    add_custom_target(copy_cfg ALL
        COMMENT "Copying cfg files")
    add_custom_command(
        TARGET copy_cfg
        COMMAND ${CMAKE_COMMAND} -E copy_directory "${PROJECT_SOURCE_DIR}/cfg"
            "${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/cfg")

    add_dependencies(testrunner copy_cfg)

    add_test(NAME testrunner COMMAND testrunner WORKING_DIRECTORY ${CMAKE_RUNTIME_OUTPUT_DIRECTORY})

endif()
