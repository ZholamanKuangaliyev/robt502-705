# This is Lab 1 :: intro to ROS
***
Exercise 0:
1HZ:
![Screenshot from 2024-01-19 12-30-37](https://github.com/ZholamanKuangaliyev/robt502-705/assets/112862577/56c47d39-78a3-4a56-9e5b-3d804c863f4f)
```
#include "ros/ros.h"
#include "std_msgs/String.h"
#include <sstream>

int main(int argc, char **argv)
{
  ros::init(argc, argv, "talker");
  ros::NodeHandle n;
  ros::Publisher chatter_pub = n.advertise<std_msgs::String>("chatter", 1000);
  ros::Rate loop_rate(1);
  int count = 0;
  int numbers [9] = {2, 0, 1, 7, 3, 8, 2, 6, 6};
  int lim = 8;
  while (ros::ok())
  {
    std_msgs::String msg;
    std::stringstream ss;
    if (count > lim) {
    	count = 0;
    }
    ss << numbers[count];
    msg.data = ss.str();
    ROS_INFO("%s", msg.data.c_str());
    chatter_pub.publish(msg);
    ros::spinOnce();
    loop_rate.sleep();
    ++count;
  }
  return 0;
}
```
50HZ:
![Screenshot from 2024-01-19 12-32-55](https://github.com/ZholamanKuangaliyev/robt502-705/assets/112862577/cd5fe6f8-d33f-43fc-a002-3e1aa5fa9f95)
```
#include "ros/ros.h"
#include "std_msgs/String.h"
#include <sstream>

int main(int argc, char **argv)
{
  ros::init(argc, argv, "talker");
  ros::NodeHandle n;
  ros::Publisher chatter_pub = n.advertise<std_msgs::String>("chatter", 1000);
  ros::Rate loop_rate(50);
  int count = 0;
  int numbers [9] = {2, 0, 1, 7, 3, 8, 2, 6, 6};
  int lim = 8;
  while (ros::ok())
  {
    std_msgs::String msg;
    std::stringstream ss;
    if (count > lim) {
    	count = 0;
    }
    ss << numbers[count];
    msg.data = ss.str();
    ROS_INFO("%s", msg.data.c_str());
    chatter_pub.publish(msg);
    ros::spinOnce();
    loop_rate.sleep();
    ++count;
  }
  return 0;
}
```
***
Exercise 1:
![Screenshot from 2024-01-19 12-02-33](https://github.com/ZholamanKuangaliyev/robt502-705/assets/112862577/c5be4929-5c7a-4960-89e4-352d8c5ca1f2)
***
Exercise 2:
![Screenshot from 2024-01-25 23-51-20](https://github.com/ZholamanKuangaliyev/robt502-705/assets/112862577/3aa67fb4-68cf-4ddc-95f5-15e498d3d3bf)

![Screenshot from 2024-01-26 00-00-01](https://github.com/ZholamanKuangaliyev/robt502-705/assets/112862577/680875c6-c1ed-44c0-a3bd-0bf85bc5e57b)
***
Exercise 3:
Subscripr.cpp code 
```
#include "ros/ros.h"
#include "turtlesim/Pose.h"
#include "geometry_msgs/Twist.h"

ros::Publisher pub;

void turtleCallback(const turtlesim::Pose::ConstPtr& msg)
{
	ROS_INFO("Turtle subscriber@[%f, %f, %f]",
	msg->x, msg->y, msg->theta);
	geometry_msgs::Twist my_vel; 
	my_vel.linear.x = 5.0; 
	my_vel.angular.z = 5.0; 
	pub.publish(my_vel);
}


int main (int argc, char **argv)
{
	// Initialize the node, setup the NodeHandle for handling the communication with the ROS
	//system
	ros::init(argc, argv, "turtlebot_subscriber");
	ros::NodeHandle nh;
	
	// Define the subscriber to turtle's position
	pub = nh.advertise<geometry_msgs::Twist>("turtle1/cmd_vel", 1);	
	ros::Subscriber sub = nh.subscribe("turtle1/pose", 1,turtleCallback);
	ros::spin();
	return 0;
}

```
Images:
![Screenshot from 2024-01-26 00-29-45](https://github.com/ZholamanKuangaliyev/robt502-705/assets/112862577/f2c3aa96-66f7-42f5-8b65-fab573cacfdf)
![Screenshot from 2024-01-26 00-29-41](https://github.com/ZholamanKuangaliyev/robt502-705/assets/112862577/8af289d1-d7e7-4af3-ae3e-ae38f11a0cfe)
***
Exercise 4:


