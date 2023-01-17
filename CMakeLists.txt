project(SpringMapConvNG)
cmake_minimum_required(VERSION 3.3)

set (CMAKE_CXX_STANDARD 11)
find_package(PkgConfig REQUIRED)
pkg_check_modules(ILU REQUIRED ILU)


set(MAPCONV_FILES
	src/CRC.cpp
	src/Image.cpp
	src/SMFMap.cpp
	src/TileStorage.cpp
)


add_executable(mapcompile src/mapcompile.cpp ${MAPCONV_FILES})
add_executable(mapdecompile src/mapdecompile.cpp ${MAPCONV_FILES})

foreach(target mapcompile mapdecompile)
	if(PREFER_STATIC_LIBS) # inside spring build
		target_compile_definitions(${target} PRIVATE IL_STATIC_LIB)
		set_target_properties(${target} PROPERTIES LINK_FLAGS "-static" )
	endif ()

	target_link_libraries(${target}
		PRIVATE
			${ILU_LINK_LIBRARIES}
	)
	target_include_directories(${target}
		PRIVATE
			${ILU_INCLUDE_DIRS}
	)
endforeach()


if (DEFINED BINDIR)
	install(TARGETS mapcompile mapdecompile RUNTIME DESTINATION ${BINDIR})
else ()
	install(TARGETS mapcompile mapcompile RUNTIME DESTINATION ${CMAKE_INSTALL_PREFIX}/bin)
endif ()

