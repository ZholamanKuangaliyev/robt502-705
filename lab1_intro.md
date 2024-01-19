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

***
Exercise 1:
![Screenshot from 2024-01-19 12-02-33](https://github.com/ZholamanKuangaliyev/robt502-705/assets/112862577/c5be4929-5c7a-4960-89e4-352d8c5ca1f2)
***
Exercise 2:
