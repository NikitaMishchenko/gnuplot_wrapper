cmake_minimum_required(VERSION 3.0.0)
project(gnuplot_wrapper VERSION 0.1.0)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

include(CTest)

#enable_testing()

include_directories (src/)


#find_package(Boost 1.67 COMPONENTS program_options REQUIRED)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED YES)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fpic")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC")

add_library (gnuplot_wrapper
             ./src/gnuplot/gnuplot_wrapper.h
             ./src/gnuplot/gnuplot_1d.h
             )

set_target_properties(gnuplot_wrapper PROPERTIES LINKER_LANGUAGE CXX)
target_link_libraries(gnuplot_wrapper
                      libboost_iostreams.a
                      libboost_system.a
                      libboost_filesystem.a)

find_package(GTest REQUIRED)
include_directories(${GTEST_INCLUDE_DIRS})

# sudo apt-get install libgsl-dev libgsl-dev libboost-dev
# libgtest-dev /usr/src/gtest make it
# sudo apt install libgnuplot-iostream-dev

#target_link_libraries(
#    gnuplot_wrapper
#)

add_executable(runTests tests/main.cpp tests/test.cpp)
target_link_libraries(runTests
                     ${GTEST_LIBRARIES}
                     gnuplot_wrapper
                     -pthread
                     -lpthread)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)

