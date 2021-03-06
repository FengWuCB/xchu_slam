cmake_minimum_required(VERSION 2.8.3)
project(xchu_slam)

# scan context要求c++14和opencv
add_compile_options(-std=c++14)

set(CMAKE_BUILD_TYPE "Debug")

find_package(catkin REQUIRED COMPONENTS
        geometry_msgs
        nav_msgs
        roscpp
        sensor_msgs
        std_msgs
        tf
        cv_bridge
        image_transport
        )

find_package(PCL REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(GTSAM REQUIRED)
find_package(OpenCV REQUIRED QUIET)

catkin_package()

include_directories(
        include
        ${catkin_INCLUDE_DIRS}
        ${PCL_INCLUDE_DIRS}
        ${EIGEN3_INCLUDE_DIRS}
        ${GTSAM_INCLUDE_DIRS}
)
link_directories(lib)

#add_library(xchu_slam
#        src/xchu_slam.cpp
#        )
#target_link_libraries(xchu_slam
#        ${catkin_LIBRARIES}
#        ${PCL_LIBRARIES}
#        ${GTSAM_LIBRARIES}
#        gtsam
#        ndt_gpu
#        ndt_cpu
#        ndt_omp
#        )


add_executable(xchu_mapping_node
        src/xchu_slam.cpp
        include/scan_context/Scancontext.cpp
        )

target_link_libraries(xchu_mapping_node
        ${catkin_LIBRARIES}
        ${PCL_LIBRARIES}
        ${GTSAM_LIBRARIES}
        ${OpenCV_LIBRARIES}
        gtsam
        ndt_gpu
        ndt_cpu
        ndt_omp
        )

add_executable(gnss_odom_node
        src/gps_odom.cpp
        include/xchu_slam/gpsTools.cpp)
target_link_libraries(gnss_odom_node ${catkin_LIBRARIES})

