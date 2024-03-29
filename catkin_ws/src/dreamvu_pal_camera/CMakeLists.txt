cmake_minimum_required(VERSION 2.8.3)
project(dreamvu_pal_camera)

find_package(catkin REQUIRED COMPONENTS
  cv_bridge
  image_transport
  roscpp
  rospy
  sensor_msgs
  std_msgs
  message_generation
  message_runtime
)

add_message_files(FILES
  BoundingBox.msg
  BoundingBoxArray.msg
)

generate_messages(
   DEPENDENCIES
   std_msgs
)

catkin_package(
   LIBRARIES dreamvu_pal_camera 
   CATKIN_DEPENDS cv_bridge image_transport roscpp rospy sensor_msgs std_msgs message_generation
  message_runtime
)

set(PAL_INCLUDE_DIR /home/argnctu/pal-camera-ros/catkin_ws/src/dreamvu_pal_camera/include )
set(PAL_LIBRARY /PAL_lib/libPAL.so  /PAL_lib/libPAL_CAMERA.so )

include_directories( 
        ${catkin_INCLUDE_DIRS} 
        ${PAL_INCLUDE_DIR}
        ${OpenCV_INCLUDE_DIRS} 

)

link_directories(
        /opt/ros/melodic/lib
     
)

set(CATKIN_LIBS cv_bridge.so image_transport.so message_filters.so class_loader.so dl.so roslib.so rospack.so python2.7.so boost_program_options.so tinyxml2.so roscpp.so boost_filesystem.so rosconsole.so rosconsole_log4cxx.so rosconsole_backend_interface.so log4cxx.so boost_regex.so xmlrpcpp.so roscpp_serialization.so rostime.so cpp_common.so boost_system.so boost_thread.so boost_chrono.so boost_date_time.so boost_atomic.so pthread.so console_bridge.so)


find_package(PkgConfig REQUIRED)
pkg_search_module(PKG_OPENCV REQUIRED opencv)

add_executable(capture src/pal_camera_node.cpp )
add_dependencies(capture ${catkin_EXPORTED_TARGETS} ${${PROJECT_NAME}_EXPORTED_TARGETS})
target_link_libraries(capture ${CATKIN_LIBS} ${PKG_OPENCV_LDFLAGS} ${V4l2_LIBRARIES} ${PAL_LIBRARY})

add_executable(detect src/person_detection_node.cpp )
add_dependencies(detect ${catkin_EXPORTED_TARGETS} ${${PROJECT_NAME}_EXPORTED_TARGETS})
target_link_libraries(detect ${CATKIN_LIBS} ${PKG_OPENCV_LDFLAGS} ${V4l2_LIBRARIES} ${PAL_LIBRARY})

add_executable(sample_detection_sub src/sample_detection_sub.cpp )
add_dependencies(sample_detection_sub ${catkin_EXPORTED_TARGETS} ${${PROJECT_NAME}_EXPORTED_TARGETS})
target_link_libraries(sample_detection_sub ${CATKIN_LIBS}  ${PKG_OPENCV_LDFLAGS} ${V4l2_LIBRARIES} ${PAL_LIBRARY})
