cmake_minimum_required(VERSION 3.14)
project(os2linux)

# ----- options ----- #

option(OS2LINUX_BUILD_TESTS "build tests" ON)
option(OS2LINUX_INSTALL "install headers and libs" ON)

if(CMAKE_HOST_SYSTEM_NAME MATCHES "Windows")
    set(OS_IS_WINDOWS True)
elseif(APPLE)
    set(OS_IS_MACOSX True)
endif()

# ----- targets ----- #

if(OS_IS_WINDOWS)
    file(GLOB OS2LINUX_SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/windows/*.c)
    add_library(os2linux_static STATIC ${OS2LINUX_SRC})
    target_include_directories(os2linux_static PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
    target_compile_features(os2linux_static PUBLIC cxx_std_11)
endif()

# ----- installations ----- #

if(OS2LINUX_INSTALL)
    if(OS_IS_MACOSX)
        install(DIRECTORY include/os2linux/macosx DESTINATION include/os2linux)
    elseif(OS_IS_WINDOWS)
        install(DIRECTORY include/os2linux/windows DESTINATION include/os2linux)
        install(TARGETS os2linux_static DESTINATION lib)
    endif()
endif()
