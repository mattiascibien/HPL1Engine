cmake_minimum_required (VERSION 2.6)
project (HPL)

set(HPL_VERSION_MAJOR 1)
set(HPL_VERSION_MINOR 0)

include_directories(
	${CMAKE_CURRENT_SOURCE_DIR}/include
	${CMAKE_CURRENT_SOURCE_DIR}/../OALWrapper/include
	${CMAKE_CURRENT_SOURCE_DIR}/../dependencies/include
)
file(GLOB_RECURSE all_sources
    ${CMAKE_CURRENT_SOURCE_DIR}/sources/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/sources/*.c
)
IF(WIN32)
	add_definitions(-D_CRT_SECURE_NO_WARNINGS)
	file(GLOB_RECURSE to_remove_all posix.cpp )
ELSE()
	file(GLOB_RECURSE to_remove_all win32.cpp Video*Theora.cpp)
ENDIF()

list(REMOVE_ITEM all_sources ${to_remove_all})

list(REMOVE_DUPLICATES all_sources)

add_library(HPL STATIC ${all_sources})

add_library(Newton STATIC IMPORTED)
set_target_properties(Newton PROPERTIES IMPORTED_LOCATION ${CMAKE_CURRENT_SOURCE_DIR}/../dependencies/lib/win32/newton.lib)

add_library(angelscript STATIC IMPORTED)
set_target_properties(angelscript PROPERTIES IMPORTED_LOCATION ${CMAKE_CURRENT_SOURCE_DIR}/../dependencies/lib/win32/angelscript.lib)

add_library(SDLmain STATIC IMPORTED)
set_target_properties(SDLmain PROPERTIES IMPORTED_LOCATION ${CMAKE_CURRENT_SOURCE_DIR}/../dependencies/lib/win32/SDLmain.lib)
