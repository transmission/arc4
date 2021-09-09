cmake_minimum_required(VERSION 3.5)
project(arc4 VERSION 1.0 LANGUAGES C)

set(ARC4_STANDALONE_BUILD OFF)
if(PROJECT_SOURCE_DIR STREQUAL CMAKE_SOURCE_DIR)
    set(ARC4_STANDALONE_BUILD ON)
endif()

option(ARC4_ENABLE_INSTALL "${PROJECT_NAME}: Enable install" ${ARC4_STANDALONE_BUILD})
option(ARC4_ENABLE_TESTS "${PROJECT_NAME}: Enable tests" ${ARC4_STANDALONE_BUILD})
option(ARC4_ENABLE_WERROR "${PROJECT_NAME}: Treat warnings as errors" ${ARC4_STANDALONE_BUILD})

if(ARC4_ENABLE_INSTALL)
    include(GNUInstallDirs)
    include(CMakePackageConfigHelpers)
endif()

if(ARC4_ENABLE_TESTS)
    enable_testing()
endif()

if(ARC4_STANDALONE_BUILD)
    if(NOT CMAKE_C_STANDARD)
        set(CMAKE_C_STANDARD 99)
        set(CMAKE_C_STANDARD_REQUIRED ON)
        set(CMAKE_C_EXTENSIONS OFF)
    endif()

    if(CMAKE_C_COMPILER_ID MATCHES "GNU|Clang")
        add_compile_options(
            -Wall
            -Wextra
            -pedantic
            $<$<BOOL:${ARC4_ENABLE_WERROR}>:-Werror>)
    elseif(CMAKE_C_COMPILER_ID STREQUAL "MSVC")
        add_compile_options(
            /W4
            $<$<BOOL:${ARC4_ENABLE_WERROR}>:/WX>)
    endif()
endif()

add_subdirectory(src)

if(ARC4_ENABLE_TESTS)
    add_subdirectory(test)
endif()

if(ARC4_ENABLE_INSTALL)
    write_basic_package_version_file(
        ${PROJECT_BINARY_DIR}/${PROJECT_NAME}-config-version.cmake
        VERSION ${PROJECT_VERSION}
        COMPATIBILITY AnyNewerVersion)

    configure_file(
        config.cmake.in
        ${PROJECT_NAME}-config.cmake
        @ONLY)

    install(
        EXPORT ${PROJECT_NAME}-targets
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
        NAMESPACE ${PROJECT_NAME}::)

    install(
        FILES
            ${PROJECT_BINARY_DIR}/${PROJECT_NAME}-config.cmake
            ${PROJECT_BINARY_DIR}/${PROJECT_NAME}-config-version.cmake
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME})

    install(
        FILES
            LICENSE
            README.md
        DESTINATION ${CMAKE_INSTALL_DOCDIR})
endif()
