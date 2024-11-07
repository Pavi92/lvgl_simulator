cmake_minimum_required(VERSION 3.13)
project(lvgl_sim C CXX)

# Find and include SDL2 library
find_package(SDL2 REQUIRED)

# Remove ARM-specific compile and linker options
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -march=native")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -march=native")

set(LV_CONF_PATH ${CMAKE_CURRENT_SOURCE_DIR}/App/include/lv_conf.h)
set(LV_CONF_BUILD_DISABLE_EXAMPLES ON CACHE BOOL "Disables building of lvgl examples")

add_subdirectory(lvgl)
target_include_directories(lvgl PUBLIC ${SDL2_INCLUDE_DIRS})
target_link_libraries(lvgl PUBLIC ${SDL2_LIBRARIES} m)

add_executable(app)
target_include_directories(app PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/App/include)

target_sources(app PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/App/src/main.c ${CMAKE_CURRENT_SOURCE_DIR}/App/src/mouse_cursor_icon.c)

target_link_libraries(app PRIVATE lvgl lvgl::demos)
