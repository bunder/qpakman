cmake_minimum_required(VERSION 2.6)

project(QPAKMAN CXX)

find_package(PNG REQUIRED)

add_definitions(${PNG_DEFINITIONS})
include_directories(${PNG_INCLUDE_DIRS} ${ZLIB_INCLUDE_DIRS})

if(UNIX)
	add_definitions(-DUNIX)
elseif(WIN32)
	add_definitions(-DWIN32)
else()
	message(SEND_ERROR "You are building on unsupported platform")
endif()

add_executable(qpakman
	arc_spec.cc u_assert.cc u_file.cc u_util.cc im_color.cc im_image.cc
	 im_gen.cc im_mip.cc im_png.cc im_tex.cc archive.cc pakfile.cc main.cc)

set_property(TARGET qpakman PROPERTY COMPILE_FLAGS -fno-rtti)

target_link_libraries(qpakman -lm ${PNG_LIBRARIES})

install(TARGETS qpakman RUNTIME DESTINATION bin)
