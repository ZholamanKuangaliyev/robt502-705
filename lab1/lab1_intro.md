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
Subscriber.cpp code 
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

Updated Subscriber.cpp code:
```
#include "ros/ros.h"
#include "turtlesim/Pose.h"
#include "geometry_msgs/Twist.h"
#include "turtlesim/Spawn.h"

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
	ros::ServiceClient client1 = nh.serviceClient<turtlesim::Spawn>("/spawn");
	turtlesim::Spawn srv1;
	srv1.request.x = 1.0;
	srv1.request.y = 5.0;
	srv1.request.theta = 0.0;
	srv1.request.name = "Turtle_JOLA";
	ros::Subscriber sub = nh.subscribe("turtle1/pose", 1,turtleCallback);
	client1.call(srv1);
	ros::spin();
	return 0;
}
```
Images:

![Screenshot from 2024-01-26 00-44-18](https://github.com/ZholamanKuangaliyev/robt502-705/assets/112862577/732eef04-90c8-4ed4-bad4-eb2b42ba3ea8)
![Screenshot from 2024-01-26 00-44-14](https://github.com/ZholamanKuangaliyev/robt502-705/assets/112862577/3fcd4b70-2fc4-4d4f-acc1-ec6f15ae055e)
***
Exercise 5:

Square Pattern:

![Screenshot from 2024-01-26 02-29-24](https://github.com/ZholamanKuangaliyev/robt502-705/assets/112862577/cbc4139f-007b-400c-b58f-eb0a722a58e8)
![Screenshot from 2024-01-26 02-29-27](https://github.com/ZholamanKuangaliyev/robt502-705/assets/112862577/75929715-d9a6-499d-8ea0-533ebb882efe)

Code of the subscriber.cpp:
```
#include "ros/ros.h"
#include "turtlesim/Pose.h"
#include "geometry_msgs/Twist.h"
#include "turtlesim/Spawn.h"
#include "turtlesim/Kill.h"
#include "turtlesim/TeleportAbsolute.h"

ros::Publisher pub;
std::string name = "Turtle_JOLA";

void turtleCallback(const turtlesim::Pose::ConstPtr& msg)
{
	ROS_INFO("Turtle subscriber@[%f, %f, %f]",
	msg->x, msg->y, msg->theta);
}

void turtleMove(double linear_speed, double angular_speed) {
	geometry_msgs::Twist my_vel; 
	my_vel.linear.x = linear_speed; 
	my_vel.angular.z = angular_speed;
	pub.publish(my_vel);
}

void squarePattern() {
	turtleMove(0.0, 1.559);
	ros::Duration(4.0).sleep();
	turtleMove(5.49 * 2, 0.0);
	// turtleRotate(90.0, 1.0);
	ros::Duration(2.0).sleep();
}

int main (int argc, char **argv)
{
	// Initialize the node, setup the NodeHandle for handling the communication with the ROS
	//system
	ros::init(argc, argv, "turtlebot_subscriber");
	ros::NodeHandle nh;
	// Define the subscriber to turtle's position
	ros::ServiceClient client1 = nh.serviceClient<turtlesim::Spawn>("/spawn");
	ros::ServiceClient client = nh.serviceClient<turtlesim::Kill>("/kill");			//kil turtle1
	turtlesim::Kill srv;															//kil turtle1
	srv.request.name = "turtle1";													//kil turtle1
	client.call(srv);																//kil turtle1
	turtlesim::Spawn srv1;
	srv1.request.x = 5.54;
	srv1.request.y = 5.54;
	srv1.request.theta = 0.0;
	srv1.request.name = name;
	client1.call(srv1);
	ros::ServiceClient tp = nh.serviceClient<turtlesim::TeleportAbsolute>("/" + name + "/teleport_absolute");			//teleport turtle_jola to corner (0.0, 0.0)
	turtlesim::TeleportAbsolute tp_srv;
	tp_srv.request.x = 0.0;
	tp_srv.request.y = 0.0;
	tp_srv.request.theta = 0.0;
	tp.call(tp_srv);
	pub = nh.advertise<geometry_msgs::Twist>("/" + name + "/cmd_vel", 1);	
	ros::Subscriber sub = nh.subscribe("/" + name + "/pose", 1, turtleCallback);
	ros::Rate rate(1);

	while (ros::ok())
	{
		squarePattern();
		ros::spinOnce();
		rate.sleep();
	}
	
	return 0;
}

Triangular Pattern:

![Screenshot from 2024-01-26 03-19-47](https://github.com/ZholamanKuangaliyev/robt502-705/assets/112862577/26230f02-d97e-4e61-ac3c-fe52e2dbe462)
![Screenshot from 2024-01-26 03-19-49](https://github.com/ZholamanKuangaliyev/robt502-705/assets/112862577/631083cb-f7ba-4a2a-9967-89514856944d)

Code:
```
#include "ros/ros.h"
#include "turtlesim/Pose.h"
#include "geometry_msgs/Twist.h"
#include "turtlesim/Spawn.h"
#include "turtlesim/Kill.h"
#include "turtlesim/TeleportAbsolute.h"

ros::Publisher pub;
std::string name = "Turtle_JOLA";

void turtleCallback(const turtlesim::Pose::ConstPtr& msg)
{
	ROS_INFO("Turtle subscriber@[%f, %f, %f]",
	msg->x, msg->y, msg->theta);
}

void turtleMove(double linear_speed, double angular_speed) {
	geometry_msgs::Twist my_vel; 
	my_vel.linear.x = linear_speed; 
	my_vel.angular.z = angular_speed;
	pub.publish(my_vel);
}

void oneTime() {
	std::cout << "onetime \n";
	turtleMove(0.0, 1.559);
	ros::Duration(4.0).sleep();
	turtleMove(5.49 * 2, 0.0);
	ros::Duration(2.0).sleep();
}

void trianglePattern() {
	for (int i = 0; i <= 2; i++) {
		switch (i) {
		case 0:
		std::cout << "case 0 \n";
			turtleMove(0.0, 1.559);
			ros::Duration(4.0).sleep();
			turtleMove(5.49 * 2, 0.0);
			ros::Duration(2.0).sleep();
			break;
		case 1:
			std::cout << "case 1 \n";
			turtleMove(0.0, 1.559 + 1.559 / 2);
			ros::Duration(4.0).sleep();
			turtleMove(15.52, 0.0);
			ros::Duration(2.0).sleep();
			break;
		case 2:
			std::cout << "case 2 \n";
			turtleMove(0.0, 1.559 + 1.559 / 2);
			ros::Duration(4.0).sleep();
			turtleMove(5.49 * 2, 0.0);
			ros::Duration(2.0).sleep();
			break;
		}	
	}
}

void squarePattern() {
	std::cout << "90 degrees \n";
	turtleMove(0.0, 1.559);
	ros::Duration(4.0).sleep();
	turtleMove(5.49 * 2, 0.0);
	ros::Duration(2.0).sleep();
}

int main (int argc, char **argv)
{
	// Initialize the node, setup the NodeHandle for handling the communication with the ROS
	//system
	ros::init(argc, argv, "turtlebot_subscriber");
	ros::NodeHandle nh;
	// Define the subscriber to turtle's position
	ros::ServiceClient client1 = nh.serviceClient<turtlesim::Spawn>("/spawn");
	ros::ServiceClient client = nh.serviceClient<turtlesim::Kill>("/kill");			//kil turtle1
	turtlesim::Kill srv;															//kil turtle1
	srv.request.name = "turtle1";													//kil turtle1
	client.call(srv);																//kil turtle1
	turtlesim::Spawn srv1;
	srv1.request.x = 5.54;
	srv1.request.y = 5.54;
	srv1.request.theta = 0.0;
	srv1.request.name = name;
	client1.call(srv1);
	ros::ServiceClient tp = nh.serviceClient<turtlesim::TeleportAbsolute>("/" + name + "/teleport_absolute");			//teleport turtle_jola to corner (0.0, 0.0)
	turtlesim::TeleportAbsolute tp_srv;
	tp_srv.request.x = 0.0;
	tp_srv.request.y = 0.0;
	tp_srv.request.theta = 0.0;
	tp.call(tp_srv);
	pub = nh.advertise<geometry_msgs::Twist>("/" + name + "/cmd_vel", 1);	
	ros::Subscriber sub = nh.subscribe("/" + name + "/pose", 1, turtleCallback);
	ros::Rate rate(1);
	oneTime();
	while (ros::ok())
	{	
		trianglePattern();
		// squarePattern();		
		ros::spinOnce();
		rate.sleep();
	}
	
	return 0;
}

```
