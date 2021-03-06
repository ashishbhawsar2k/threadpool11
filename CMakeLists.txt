CMAKE_MINIMUM_REQUIRED(VERSION 2.8)
PROJECT(threadpool11)

# SET a default build type if none was specified
if(NOT CMAKE_BUILD_TYPE)
  MESSAGE(STATUS "Setting build type to 'Debug' as none was specified.")
  SET(CMAKE_BUILD_TYPE Release CACHE STRING "Choose the type of build." FORCE)
  # SET the possible values of build type for cmake-gui
endif()

SET_PROPERTY(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Release" "Debug" "Perf")

if(CMAKE_COMPILER_IS_GNUCXX)
	SET(CMAKE_CXX_FLAGS "-std=c++11")
	SET(CMAKE_CXX_FLAGS_DEBUG "-g")
	SET(CMAKE_CXX_FLAGS_RELEASE "-O3 -DNDEBUG -flto -fuse-linker-plugin")
	SET(CMAKE_CXX_FLAGS_PERF "-O3 -DNDEBUG -flto -fuse-linker-plugin -Wno-inline -pg")
	SET(CMAKE_SHARED_LINKER_FLAGS_RELEASE "-flto -fuse-linker-plugin") 
	SET(CMAKE_SHARED_LINKER_FLAGS_PERF "-flto -fuse-linker-plugin -pg") 
elseif(MSVC)
	SET(CMAKE_CXX_FLAGS_DEBUG "/Od")
	SET(CMAKE_CXX_FLAGS_RELEASE "/O3 /GL")
	SET(CMAKE_SHARED_LINKER_FLAGS_RELEASE "/LTCG")
endif()

SET(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/build/lib)
SET(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/build/lib)
SET(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/build/bin)

ADD_SUBDIRECTORY(threadpool11)
ADD_SUBDIRECTORY(threadpool11_demo)

FILE(COPY ./threadpool11/include DESTINATION ./build)
