set(SQSTDLIB_SRC sqstdaux.cpp
                 sqstdblob.cpp
                 sqstdio.cpp
                 sqstdmath.cpp
                 sqstdrex.cpp
                 sqstdstream.cpp
                 sqstdstring.cpp
                 sqstdsystem.cpp)

add_library(sqstdlib ${SQSTDLIB_SRC})
target_link_libraries(sqstdlib squirrel)

if(LONG_OUTPUT_NAMES)
    set_target_properties(sqstdlib PROPERTIES OUTPUT_NAME sqstdlib3)
endif()


install(
    TARGETS
      sqstdlib
      ${INSTALL_TARGETS_DEFAULT_ARGS}
)
