cmake_minimum_required(VERSION 3.17)
project(lua-protobuf C)

set(LUA_INCLUDE_DIR "" CACHE PATH "Lua 头文件目录")
set(LUA_LIBRARY "" CACHE FILEPATH "Lua 导入库或静态库路径")

set(PB_SOURCES pb.c)

if(WIN32)
    list(APPEND PB_SOURCES pb.def)
endif()

add_library(pb SHARED ${PB_SOURCES})
set_target_properties(pb PROPERTIES PREFIX "")

if(LUA_INCLUDE_DIR)
    target_include_directories(pb PRIVATE "${LUA_INCLUDE_DIR}")
endif()

if(LUA_LIBRARY)
    target_link_libraries(pb PRIVATE "${LUA_LIBRARY}")
endif()
