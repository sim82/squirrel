if(CMAKE_COMPILER_IS_GNUCXX)
set_source_files_properties(sq.c PROPERTIES COMPILE_FLAGS -std=c99)
endif()
add_executable(sq sq.c)
set_target_properties(sq PROPERTIES LINKER_LANGUAGE C)
target_link_libraries(sq squirrel sqstdlib)


install(TARGETS sq ${INSTALL_TARGETS_DEFAULT_ARGS})


#install(FILES sq DESTINATION ${CMAKE_INSTALL_BINDIR})


if(LONG_OUTPUT_NAMES)
    set_target_properties(sq PROPERTIES OUTPUT_NAME squirrel3)
endif()
