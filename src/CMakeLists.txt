add_library(arc4 STATIC
    arc4.c
    arc4.h)

if(NOT ARC4_STANDALONE_BUILD)
    add_library(${PROJECT_NAME}::arc4 ALIAS arc4)
endif()

target_include_directories(arc4
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)

if(ARC4_ENABLE_INSTALL)
    install(
        TARGETS arc4
        EXPORT ${PROJECT_NAME}-targets
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})

    install(
        FILES arc4.h
        DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
endif()
