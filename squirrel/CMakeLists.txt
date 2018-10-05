set(SQUIRREL_SRC sqapi.cpp
                 sqbaselib.cpp
                 sqclass.cpp
                 sqcompiler.cpp
                 sqdebug.cpp
                 sqfuncstate.cpp
                 sqlexer.cpp
                 sqmem.cpp
                 sqobject.cpp
                 sqstate.cpp
                 sqtable.cpp
                 sqvm.cpp)

add_library(squirrel ${SQUIRREL_SRC})


if(LONG_OUTPUT_NAMES)
    set_target_properties(squirrel PROPERTIES OUTPUT_NAME squirrel3)
endif()

if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    target_compile_definitions( squirrel PUBLIC -D_SQ64)
endif()


install(
    TARGETS
      squirrel
      ${INSTALL_TARGETS_DEFAULT_ARGS}
)


target_include_directories(squirrel INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
  $<INSTALL_INTERFACE:include>
)


set(SQ_PUB_HEADERS ../include/sqconfig.h
                   ../include/sqstdaux.h
                   ../include/sqstdblob.h
                   ../include/sqstdio.h
                   ../include/sqstdmath.h
                   ../include/sqstdstring.h
                   ../include/sqstdsystem.h
                   ../include/squirrel.h)
install(FILES ${SQ_PUB_HEADERS} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/squirrel)

#install(FILES ${capnp_headers}  DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/squirrel")
