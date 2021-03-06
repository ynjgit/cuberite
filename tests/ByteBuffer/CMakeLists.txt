cmake_minimum_required (VERSION 2.6)

enable_testing()
add_definitions(-DTEST_GLOBALS=1)

include_directories(${CMAKE_SOURCE_DIR}/src/)

add_definitions(-DTEST_GLOBALS=1)

set (SHARED_SRCS
	${CMAKE_SOURCE_DIR}/src/ByteBuffer.cpp
	${CMAKE_SOURCE_DIR}/src/OSSupport/StackTrace.cpp
)

set (SHARED_HDRS
	${CMAKE_SOURCE_DIR}/src/ByteBuffer.h
	${CMAKE_SOURCE_DIR}/src/OSSupport/StackTrace.h
)

set (SRCS
	ByteBufferTest.cpp
)

if (MSVC)
	# Add the MSVC-specific LeakFinder / StackTracer sources:
	list (APPEND SHARED_SRCS ${CMAKE_SOURCE_DIR}/src/LeakFinder.cpp ${CMAKE_SOURCE_DIR}/src/StackWalker.cpp)
	list (APPEND SHARED_HDRS ${CMAKE_SOURCE_DIR}/src/LeakFinder.h   ${CMAKE_SOURCE_DIR}/src/StackWalker.h)
endif()

source_group("Shared" FILES ${SHARED_SRCS} ${SHARED_HDRS})
source_group("Sources" FILES ${SRCS})
add_executable(ByteBuffer-exe ${SRCS} ${SHARED_SRCS} ${SHARED_HDRS})
if (WIN32)
	target_link_libraries(ByteBuffer-exe ws2_32)
endif()
add_test(NAME ByteBuffer-test COMMAND ByteBuffer-exe)





# Put the projects into solution folders (MSVC):
set_target_properties(
	ByteBuffer-exe
	PROPERTIES FOLDER Tests
)
