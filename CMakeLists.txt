CMAKE_MINIMUM_REQUIRED (VERSION 3.6)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
project(IfcPlusPlus)

# Set a default build type if none was specified https://blog.kitware.com/cmake-and-the-default-build-type/
set(default_build_type "Release")
 
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
	message(STATUS "Setting build type to '${default_build_type}' as none was specified.")
  
	set(CMAKE_BUILD_TYPE "${default_build_type}" CACHE STRING "Choose the type of build." FORCE)
  
	# Set the possible values of build type for cmake-gui
	set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()
message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")

project(IFCPP)
message(STATUS "------------------------------------------------------------------------")
IF(NOT WIN32)
    IF("${CMAKE_BUILD_TYPE}" MATCHES "Debug")
        IF(CMAKE_CXX_COMPILER_ID STREQUAL "GNU" OR CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
            SET_PROPERTY(DIRECTORY APPEND PROPERTY
                COMPILE_DEFINITIONS $<$<CONFIG:Debug>:_DEBUG>
            )
        ENDIF()
    ENDIF()
    set(CMAKE_POSITION_INDEPENDENT_CODE ON)
ELSE(NOT WIN32)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /MP")
ENDIF(NOT WIN32)

set(IFCPP_CONFIG_DIR "share/IFCPP/cmake")
ADD_SUBDIRECTORY (IfcPlusPlus)
  
IF(BUILD_CONSOLE_APPLICATION)
  ADD_SUBDIRECTORY (examples/CreateIfcWallAndWriteFile)
  ADD_SUBDIRECTORY (examples/LoadFileExample)
ENDIF()
