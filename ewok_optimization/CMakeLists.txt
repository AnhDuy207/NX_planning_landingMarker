cmake_minimum_required(VERSION 2.8.3)
project(ewok_optimization)

set(CMAKE_MODULE_PATH "${CMAKE_MODULE_PATH}" "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

add_definitions(-std=c++11 -g -O3 )

find_package(catkin_simple REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(SuiteSparse REQUIRED)
find_package(Boost COMPONENTS thread REQUIRED)

include_directories(${EIGEN3_INCLUDE_DIR} ${CHOLMOD_INCLUDE_DIR})
set(CHOLMOD_LIBRARY "/usr/lib/aarch64-linux-gnu/libcholmod.so")
catkin_simple()

cs_add_executable(spline_optimization_example src/spline_optimization_example.cpp)
target_link_libraries(spline_optimization_example nlopt)

cs_add_executable(spline_optimization_example2 src/spline_optimization_example2.cpp)
target_link_libraries(spline_optimization_example2 nlopt)

cs_add_executable(spline_optimization_example3 src/spline_optimization_example3.cpp)
target_link_libraries(spline_optimization_example3 ${CHOLMOD_LIBRARY} nlopt)

cs_add_executable(rrt_optimization_single src/rrt_optimization_single.cpp)
target_link_libraries(rrt_optimization_single ${CHOLMOD_LIBRARY} ${Eigen3_LIBS} nlopt Boost::thread)


cs_add_executable(polynomial_optimization_example src/polynomial_optimization_example.cpp)
target_link_libraries(polynomial_optimization_example ${CHOLMOD_LIBRARY})

catkin_add_gtest(test_uniform_bspline_3d_optimization test/uniform-bspline-3d-optimization-test.cpp)
target_link_libraries(test_uniform_bspline_3d_optimization nlopt)

cs_install()
cs_export()
