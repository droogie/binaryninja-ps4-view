cmake_minimum_required(VERSION 3.9 FATAL_ERROR)

project(view_ps4)

if((NOT BN_API_PATH) AND (NOT BN_INTERNAL_BUILD))
        set(BN_API_PATH $ENV{BN_API_PATH})
        if(NOT BN_API_PATH)
                message(FATAL_ERROR "Provide path to Binary Ninja API source in BN_API_PATH")
        endif()
endif()
if(NOT BN_INTERNAL_BUILD)
        add_subdirectory(${BN_API_PATH} ${PROJECT_BINARY_DIR}/api)
endif()

file(GLOB SOURCES
	*.cpp
	*.h)

if(DEMO)
	add_library(view_ps4 STATIC ${SOURCES})
else()
	add_library(view_ps4 SHARED ${SOURCES})
endif()

target_link_libraries(view_ps4 binaryninjaapi)

set_target_properties(view_ps4 PROPERTIES
    CXX_STANDARD 17
	CXX_VISIBILITY_PRESET hidden
	CXX_STANDARD_REQUIRED ON
    VISIBILITY_INLINES_HIDDEN ON
	POSITION_INDEPENDENT_CODE ON)

if(BN_INTERNAL_BUILD)
	plugin_rpath(view_ps4)
	set_target_properties(view_ps4 PROPERTIES
		LIBRARY_OUTPUT_DIRECTORY ${BN_CORE_PLUGIN_DIR}
		RUNTIME_OUTPUT_DIRECTORY ${BN_CORE_PLUGIN_DIR})
endif()
