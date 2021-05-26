# Setup ROS communication node between Jetson Nano and Arudino
## Initial Setup
## Jetson Nano
```bash
# Deb package that supports serial communication
sudo apt-get install ros-melodic-rosserial-arduino
sudo apt-get install ros-melodic-rosserial
```
### Arudino Setup
Sketch->Include lib->Manage Lib->rosserial

### Arudino Test code compile and load
```
/* 
 * rosserial Subscriber Example
 * Blinks an LED on callback
 */

#include <ros.h>
#include <std_msgs/Empty.h>

ros::NodeHandle  nh;

void messageCb( const std_msgs::Empty& toggle_msg){
  digitalWrite(LED_BUILTIN, HIGH-digitalRead(LED_BUILTIN));   // blink the led
}

ros::Subscriber<std_msgs::Empty> sub("toggle_led", &messageCb );

void setup()
{ 
  pinMode(LED_BUILTIN, OUTPUT);
  nh.initNode();
  nh.subscribe(sub);
}

void loop()
{  
  nh.spinOnce();
  delay(1);
}

```

### Test
1. connect Arudino and Jetson Nano through USB
2. In Jetson Nano terminal
```
# Terminal 1
sudo chmod 666 /dev/ttyACM0

rosrun rosserial_python serial_node.py /dev/ttyACM0

# Terminal 2
rostopic pub toggle_led std_msgs/Empty --once
```

