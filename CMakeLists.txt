cmake_minimum_required(VERSION 3.10)
project(lvgl)
set(CMAKE_C_STANDARD 11)#C11
set(CMAKE_CXX_STANDARD 17)#C17
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(APP_DIR "USER/App")
set(LIB_DIR "Libraries")


file(GLOB_RECURSE UTILS_SRCS
        "${APP_DIR}/Utils/*.c"
        "${APP_DIR}/Utils/*.cpp"
        )


file(GLOB SRCS
        "${APP_DIR}/Common/DataProc/*.cpp"
        "${APP_DIR}/Common/Music/*.cpp"
        "${APP_DIR}/Pages/*.cpp"
        "${APP_DIR}/Pages/*/*.cpp"
        "${APP_DIR}/Resource/*.cpp"
        "${APP_DIR}/Resource/*/*.c"
        ${UTILS_SRCS}
        "lv_fs_if/*.c"
        )

file(GLOB HAL_SRCS
        "HAL/*.cpp"
        )

#INCLUDE_DIRECTORIES(${PROJECT_SOURCE_DIR})

include_directories(
        "./"
        "HAL"
        "${LIB_DIR}"
        "${APP_DIR}"
        "${APP_DIR}/Utils/ArduinoJson/src"
)
file(GLOB_RECURSE INCLUDES
        "lv_fs_if/*.h"
        "${APP_DIR}/*.h"
        )

add_subdirectory(lvgl)
add_subdirectory(lv_drivers)

SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
SET(CMAKE_CXX_FLAGS "-O3")

#find_package(SDL2 REQUIRED SDL2)
#include_directories(${SDL2_INCLUDE_DIRS})
add_executable(main
        main.cpp
        ${SRCS}
        "${APP_DIR}/App.cpp"
        ${SOURCES}
        ${INCLUDES}
        ${HAL_SRCS}
#        "${LIB_DIR}/TinyGPSPlus/src/TinyGPS++.cpp"
#        "${LIB_DIR}/ButtonEvent/ButtonEvent.cpp"
        )


add_compile_definitions(LV_CONF_INCLUDE_SIMPLE)
target_link_libraries(main PRIVATE lvgl  lvgl::drivers)
add_custom_target(run COMMAND ${EXECUTABLE_OUTPUT_PATH}/main)
