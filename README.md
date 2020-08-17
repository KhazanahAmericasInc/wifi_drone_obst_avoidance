# Collision Avoidance for WiFi Drones

Most drones with collision avoidance incorporate some degree of computer vision and artificial intelligence. This project aims to extend the autonomous and safety capabilities of smaller and more affordable consumer drones (i.e. DJI Tello ~$100) with a lightweight (about 40g), low-cost (about $40) collision avoidance system.

The collision avoidance system utilizes multiple VL53L0X TOF sensors connected to a Raspberry Pi Zero W. Distance measurements are communicated over Bluetooth to a host computer. Simultaneously, the computer send actuation commands to the drone over its WiFi.

VL53L0X readings and drone IMU data are fused using a kalman filter.
A PD controller commands specific velocities to the drone.

## Hardware Assembly

https://www.instructables.com/id/VL53L0X-Sensor-System/

## Setup Packages

### Raspberry Pi:


```
sudo apt-get update
sudo apt-get install python-pip python-dev ipython build-essential

sudo apt-get install bluetooth libbluetooth-dev
```

VL53L0x Python Library: https://github.com/pimoroni/VL53L0X_rasp_python

```
cd your_git_directory
git clone https://github.com/pimoroni/VL53L0X_rasp_python.git
cd VL53L0X_rasp_python
make

```

PyBluez 0.22: https://github.com/pybluez/pybluez
```
sudo pip install pybluez=0.22
```



### Linux PC:

ROS Kinetic: http://wiki.ros.org/kinetic/Installation/Ubuntu

Tellopy (build from source): https://github.com/hanyazou/TelloPy

PyBluez 0.22: https://github.com/pybluez/pybluez
```
sudo apt-get install bluetooth libbluetooth-dev
sudo pip install pybluez=0.22
```


## Building The Repository


```
mkdir -p ~/catkin_ws_tello/src/

cd ~/catkin_ws_tello/src/

git clone https://github.com/aaaaronlin/tello_collision_avoidance/

cd ..

catkin build
```

## ROS nodes and topics

![rqt_graph](img/rosgraph.png)

## Setting up Wireless Communication

### PiZero
Enable Bluetooth
Disable WiFi (for interference purposes)

Running a script on startup:

copy /board/sensor_system.py to /Desktop on PiZero

change the bluetooth address to your PC address, as well as GPIO pins used

```
cd
sudo nano /etc/rc.local
```

add the line:

```
python2 /home/pi/Desktop/sensor_system.py &
```

This will allow the script to run when the PiZero is externally charged by the drone battery.

### PC
Enable Bluetooth
Enable WiFi (for drone AP)
