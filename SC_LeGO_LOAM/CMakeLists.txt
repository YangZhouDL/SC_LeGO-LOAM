cmake_minimum_required(VERSION 2.8.3)
project(sc_lego_loam)

find_package(catkin_simple REQUIRED)

#uncomment next line to use OpenCV library
#find_package(OpenCV REQUIRED)

#uncomment the next 2 lines to use the point-cloud library
#find_package(PCL 1.7 REQUIRED)
#include_directories(${PCL_INCLUDE_DIRS})


#uncomment the following 4 lines to use the Eigen library
#find_package(cmake_modules REQUIRED)
#find_package(Eigen3 REQUIRED)
#include_directories(${EIGEN3_INCLUDE_DIR})
#add_definitions(${EIGEN_DEFINITIONS})

find_package (Eigen3 3.3 REQUIRED NO_MODULE)
find_package(GTSAM REQUIRED QUIET)
find_package(PCL REQUIRED QUIET)
find_package(OpenCV REQUIRED QUIET)

include_directories(
	include
	${PCL_INCLUDE_DIRS}
	${OpenCV_INCLUDE_DIRS}
	${GTSAM_INCLUDE_DIR}
)

link_directories(
	include
	${OpenCV_LIBRARY_DIRS}
	${PCL_LIBRARY_DIRS}
	${GTSAM_LIBRARY_DIRS}
)

catkin_simple()

# example boost usage
# find_package(Boost REQUIRED COMPONENTS system thread)

# C++0x support - not quite the same as final C++11!
# use carefully;  can interfere with point-cloud library
# SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
set(CMAKE_CXX_STANDARD 14)

# Libraries: uncomment the following and edit arguments to create a new library
# cs_add_library(my_lib src/my_lib.cpp)   

# Executables: uncomment the following and edit arguments to compile new nodes
# may add more of these lines for more nodes from the same package
# cs_add_executable(example src/example.cpp)

#the following is required, if desire to link a node in this package with a library created in this same package
# edit the arguments to reference the named node and named library within this package
# target_link_library(example my_lib)

cs_add_executable(imageProjection src/imageProjection.cpp)
target_link_libraries(imageProjection ${PCL_LIBRARIES} ${OpenCV_LIBRARIES})

cs_add_executable(featureAssociation src/featureAssociation.cpp)
target_link_libraries(featureAssociation ${PCL_LIBRARIES} ${OpenCV_LIBRARIES})

cs_add_executable(mapOptmization 
    src/mapOptmization.cpp
    src/Scancontext.cpp
)
target_link_libraries(mapOptmization 
    ${PCL_LIBRARIES} 
    ${OpenCV_LIBRARIES} 
    gtsam
    Eigen3::Eigen
)

cs_add_executable(transformFusion src/transformFusion.cpp)
target_link_libraries(transformFusion ${PCL_LIBRARIES} ${OpenCV_LIBRARIES})

cs_install()
cs_export()
    