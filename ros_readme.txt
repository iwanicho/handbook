[ROS]

# Install ROS -> source opt/ros/melodic/setup.bash

# catkin_make (whenever build a package and edit package (C++ code, CMakeList.txt, package.xml))
# source ~/catkin_ws/devel/setup.bash (once)

# Workspace -> Some Packages (functions) -> Nodes

# Packages is in ~catkin_ws/src/

# "CMakeList.txt" file describes how to build the code and where to install it to. It is also the input to the CMake build system for building software packages.

# "package.xml" is used to describe the package and set its dependencies

# Make a package:
   1) cd ~/catkin_ws/src
   2) catkin_create_pkg <package_name> <dependencies_1> <dependencies_2> etc.
   3) @~catkin_ws/ -> catkin_make

# To run a node:
   a) cd ~/scripts -> python2 <filename>
   b) cd ~/scripts -> ./<filename (python)>
   c) rosrun <package_name> <filename (python)/node name (cpp)>

# Create a custom message (http://wiki.ros.org/ROS/Tutorials/CreatingMsgAndSrv#Creating_a_msg)
   1) roscd <package_name>
   2) mkdir msg
   3) touch message_name.msg
   4) edit the message file
   5) open "package.xml" then add
             <build_depend>message_generation</build_depend>
             <exec_depend>message_runtime</exec_depend>
   6) open "CMakeList.txt" then add
              find_package(catkin REQUIRED COMPONENTS
                          ...
                          message_generation
                          ...)
              catkin_package(
                          ...
                          CATKIN_DEPENDS message_runtime ...
                          ...)
              add_message_files(
                         FILES
                         message.msg)
              generate_messages(
                         DEPENDENCIES
                         std_msgs)

# Create a publisher node
   Python:
   1) rospy.init_node('node_name', anonymous=True)
   2) pub = rospy.Publisher('topic_name', data_type, queue_size=buffer_size)
   3) rate = rospy.Rate(1) #in Hz
   4) while not rospy.is_shutdown( ) :
             #add some command
             pub.publish(var)
             rate.sleep
   C++:
   1) int main (int argc, char **argv) { }
   2) ros::init(argc, argv, "node_name");
   3) ros::NodeHandle nh;
   4) ros::Rate rate(1) //in Hz
   5) ros::Publisher pub = nh.advertise<data_type>("topic_name",buffer size)
   6) while(ros::ok( )) {
             //add some command
            pub.publish(var);
            rate.sleep( );
       }

# Create a subscriber
   Python:
   1) rospy.init_node('node_name', anonymous=True)
   2) rospy.Subscriber("topic_name", data_type, callback_function)
   3) rospy.spin()
   4) def callback_function
   C++:
   1) int main (int argc, char **argv) { }
   2) ros::init(argc, argv, "node_name");
   3) ros::NodeHandle nh;
   4) ros::Subscriber sub = nh.subscriber("topic_name", buffer size, callback_function);
   5) ros::spin()
   6) define "callback_function(const message_type &var)"

# Create a executable cpp node
    1) make the cpp code
    2) open "CMakeList.txt" then add
             add_executable(node_name <cpp source code>)
             target_link_libraries (node_name ${catkin_LIBRARIES})
             add_dependencies(node_name ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

# Create a custom service (http://wiki.ros.org/ROS/Tutorials/CreatingMsgAndSrv#Creating_a_msg)
   1) roscd <package_name>
   2) mkdir srv
   3) touch service_name.srv
   4) edit the service file
              int64 a
              string b
              ---
              float32 responseVar
   5) open "package.xml" then add
             <build_depend>message_generation</build_depend>
             <exec_depend>message_runtime</exec_depend>
   6) open "CMakeList.txt" then add
              find_package(catkin REQUIRED COMPONENTS
                          ...
                          message_generation
                          ...)
              catkin_package(
                          ...
                          CATKIN_DEPENDS message_runtime ...
                          ...)
              add_service_files(
                         FILES
                         ServiceType.srv)

# Create the client:
   Python:
    1) from package_name.srv import service_type
        from package_name.srv import service_typeRequest
        from package_name.srv import service_typeResponse
    2) def service_client(x, y):
              rospy.wait_for_service('service_name')
              try:
                  func = rospy.ServiceProxy('service_name', service_type)
                  resp = func(x, y)
                  return resp.responseVar
              except rospy.ServiceException(e):
                  print("Error")
    3) if _name_ == '_main_':
              rospy.init_node("node_name")
              sum = service_client(2, 4)
              ropy.loginfo("The response: %.2f", sum);
   C++:
    1) #include "ros/ros.h"
        #include "package_name/ServiceType.h"
    2) int main(int argc, char **argv) {
            ros::init(argc, argv, "node_name");
            ros::NodeHandle nh;
            ros::ServiceClient client = nh.serviceClient<package_name::ServiceTypes>("service_name");

            package_name::ServiceTypes srv;
            srv.request.a = 2;
            
            if (client.call(srv)) {
                   ROS_INFO("The response: %.2f", srv.response.responseVar);
            } else {
                   ROS_INFO("Failed.");
                   return 1;
            }
            return 0;
       }

# Create the server:
   Python:
    1) from package_name.srv import ServiceType
        from package_name.srv import ServiceTypeRequest
        from package_name.srv import ServiceTypeResponse
    2) def handle_function(req):
               #add some commands
               #example:
               sum = ServiceTypeResponse(req.a + 20.0)
               return sum
    3) rospy.init_node('node_name')
    4) object = rospy.Service('service_name', ServiceType, handle_function)
    5) rospy.spin( )
   C++:
    1) #include "ros/ros.h"
        #include "package_name/ServiceType.h"
    2) bool handle_function (package_name::ServiceType::Request &req,
                                            package_name::ServiceType::Response &res) {
            res.responseVar = req.a + 20.0;
            return true
        }
    3) int main(int arc, char **argv) {
            ros::init(argc, argv, "node_name");
            ros::NodeHandle nh;
            ros::ServiceServer object = nh.advertiseService("service_name",handle_function);
            ros::spin( );

            //add some commands

            return 0;
         }