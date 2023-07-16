project(SpringMapConvNG)
cmake_minimum_required(VERSION 3.3)

set (CMAKE_CXX_STANDARD 11)

find_package(DevIL REQUIRED)

set(MAPCONV_FILES
	src/CRC.cpp
	src/Image.cpp
	src/SMFMap.cpp
	src/TileStorage.cpp
)


add_executable(mapcompile src/mapcompile.cpp ${MAPCONV_FILES})
add_executable(mapdecompile src/mapdecompile.cpp ${MAPCONV_FILES})

foreach(target mapcompile mapdecompile)
	target_include_directories(${target} PRIVATE ${IL_INCLUDE_DIRS})

	if(PREFER_STATIC_LIBS AND ILU_STATIC_LIBRARIES)
		set_target_properties(${target} PROPERTIES LINK_FLAGS "-static")
		target_link_libraries(${target} PRIVATE ${ILU_STATIC_LIBRARIES})
	else()
		target_link_libraries(${target} PRIVATE ${IL_LINK_LIBRARIES} ${ILU_LINK_LIBRARIES})
	endif()
endforeach()


if (DEFINED BINDIR)
	install(TARGETS mapcompile mapdecompile RUNTIME DESTINATION ${BINDIR})
else ()
	install(TARGETS mapcompile mapcompile RUNTIME DESTINATION ${CMAKE_INSTALL_PREFIX}/bin)
endif ()

