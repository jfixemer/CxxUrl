cmake_minimum_required(VERSION 3.4)
project(cxxurl)

set(CMAKE_CXX_STANDARD 11)

add_definitions(-D_GNU_SOURCE -Wall -Wextra)

#*********************************************************
# determine platform
#*********************************************************
set(PLATFORM UNKNOWN)

if(${CMAKE_SYSTEM_NAME} STREQUAL "Linux")
    set(PLATFORM LINUX)
    include(GNUInstallDirs)
else ()
    # Sorry I don't have the time and environments to test the platforms
    # the baseline author provided.
    message(FATAL_ERROR "Untested/Unsupported Platform")
endif()

message("Platform: " ${CMAKE_SYSTEM_NAME} "-" ${PLATFORM})

set(HEADERS 
    ${CMAKE_CURRENT_SOURCE_DIR}/url.hpp)

set(SOURCES 
    ${HEADERS} 
    ${CMAKE_CURRENT_SOURCE_DIR}/url.cpp)

add_library(${PROJECT_NAME} SHARED ${SOURCES})
add_library(${PROJECT_NAME}_static STATIC ${SOURCES})
SET_TARGET_PROPERTIES(${PROJECT_NAME}_static PROPERTIES OUTPUT_NAME ${PROJECT_NAME})

set_target_properties(${PROJECT_NAME} PROPERTIES SOVERSION 1)
set_target_properties(${PROJECT_NAME} PROPERTIES PUBLIC_HEADER url.hpp)

# install headers
install(FILES ${HEADERS} DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/cxxurl")

install(TARGETS ${PROJECT_NAME} ${PROJECT_NAME}_static
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})
