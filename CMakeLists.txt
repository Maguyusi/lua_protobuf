cmake_minimum_required(VERSION 3.20)
project(lua-protobuf C)

set(LUA_INCLUDE_DIR "" CACHE PATH "Lua header directory")
set(LUA_LIBRARY "" CACHE FILEPATH "Lua import or static library path")

if(NOT LUA_INCLUDE_DIR)
  message(FATAL_ERROR "LUA_INCLUDE_DIR is required")
endif()

if(NOT LUA_LIBRARY)
  message(FATAL_ERROR "LUA_LIBRARY is required")
endif()

set(PB_SOURCES pb.c)
if(WIN32 AND BUILD_SHARED_LIBS)
  list(APPEND PB_SOURCES pb.def)
endif()

add_library(pb ${PB_SOURCES})
target_include_directories(pb PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}" "${LUA_INCLUDE_DIR}")
target_link_libraries(pb PRIVATE "${LUA_LIBRARY}")
set_target_properties(pb PROPERTIES
  PREFIX ""
  POSITION_INDEPENDENT_CODE ON
)

install(
  TARGETS pb
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
)
install(FILES pb.h protoc.lua DESTINATION include)
