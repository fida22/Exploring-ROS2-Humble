# 🤖 Exploring ROS2 Humble

A hands-on learning repository documenting the journey of exploring **ROS 2 Humble Hawksbill** — from installation and core concepts to building a fully simulated 4-wheeled differential drive robot.

---

## 📁 Repository Structure

```
Exploring-ROS2-Humble/
├── src/
│   ├── my_robot_controller/          # Week 1 — Publisher/Subscriber package
│   │   ├── my_robot_controller/
│   │   │   ├── publisher_node.py     # Publishes incrementing integers to /number
│   │   │   └── subscriber_node.py   # Subscribes to /number and logs squared values
│   │   ├── package.xml
│   │   ├── setup.py
│   │   └── setup.cfg
│   │
│   └── four_wheel_robot/             # Week 2 — 4-Wheeled Robot Simulation package
│       ├── four_wheel_robot/
│       │   └── obstacle_stop_node.py # Auto stops robot when obstacle < 0.5m
│       ├── urdf/
│       │   └── four_wheel_robot.urdf.xacro
│       ├── worlds/
│       │   └── final_wall.world
│       ├── launch/
│       │   └── simulate.launch.py
│       ├── package.xml
│       ├── setup.py
│       └── setup.cfg
│
├── images/                           # Screenshots and diagrams
├── Installation-&-Basics.md         # ROS2 installation, workspace & node creation guide
├── ROS2 Communication.md            # DDS, peer-to-peer discovery, why ROS Master was dropped
├── four_wheel_robot_pckg.md         # Full documentation for the robot simulation package
└── .gitignore
```

---

## 📚 Documentation

| File | Description |
|------|-------------|
| [`Installation-&-Basics.md`](./Installation-%26-Basics.md) | Step-by-step ROS2 installation, workspace setup, creating packages & nodes, Publisher/Subscriber basics |
| [`ROS2 Communication.md`](./ROS2%20Communication.md) | Deep dive into DDS, peer-to-peer discovery, and why ROS2 dropped the ROS Master |
| [`four_wheel_robot_pckg.md`](./four_wheel_robot_pckg.md) | Full documentation for the 4-wheeled robot simulation with LIDAR, camera, and obstacle avoidance |

---

## 🚀 Topics Covered

### Week 1 — ROS2 Basics & Publisher/Subscriber
- Installing ROS2 Humble on Ubuntu
- Understanding the **Node → Package → Workspace** hierarchy
- Creating a ROS2 workspace with `colcon build`
- Writing and registering ROS2 nodes using OOP in Python
- Publisher/Subscriber pattern — a publisher sends incrementing integers to `/number`, and a subscriber listens and logs the squared value

### ROS2 Communication
- What is **DDS (Data Distribution Service)** and how it works
- Peer-to-peer node discovery (no central server needed)
- Why ROS2 removed the **ROS Master**: eliminates single point of failure, better scalability, real-time QoS support

### Week 2 — Four-Wheel Robot Simulation
- Simulated 4-wheeled differential drive robot in **Gazebo Classic**
- Robot equipped with **LIDAR** and **Camera** sensors
- Automatic **obstacle stop** when wall is within 0.5m
- Teleoperated via `teleop_twist_keyboard`
- Visualized in **RViz** using URDF/Xacro

---

## 🛠️ Prerequisites

- Ubuntu 22.04
- [ROS2 Humble Hawksbill](https://docs.ros.org/en/humble/Installation.html)
- Gazebo Classic
- Python 3
- `teleop_twist_keyboard` package

---

## ⚙️ Setup — Clone the Repo

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
git clone https://github.com/fida22/Exploring-ROS2-Humble.git
```

---

## 📦 Package 1 — Week 1: Publisher & Subscriber

### Build

```bash
cd ~/ros2_ws
colcon build --packages-select my_robot_controller
source install/setup.bash
```

### Run

Open two terminals (source the workspace in each):

**Terminal 1 — Publisher Node:**
```bash
ros2 run my_robot_controller publisher_node
```

**Terminal 2 — Subscriber Node:**
```bash
ros2 run my_robot_controller subscriber_node
```

The publisher sends incrementing integers to `/number`, and the subscriber prints the squared value of each received number.

---

## 📦 Package 2 — Four-Wheel Differential Drive Robot

### Build

```bash
cd ~/ros2_ws
colcon build --packages-select four_wheel_robot
source install/setup.bash
```

### Run the Gazebo Simulation

```bash
ros2 launch four_wheel_robot simulation.launch.py
```

This will launch:
- Gazebo world with a wall
- Robot spawned in the world
- Robot State Publisher
- Teleop node
- Obstacle Stop Node

### Visualize in RViz

```bash
ros2 launch urdf_tutorial display.launch.py \
  model:=/home/fida/marstask_ros2_ws/src/four_wheel_robot/urdf/four_wheel_robot.urdf.xacro
```

> **Note:** Update the model path to match your local workspace directory if different.

---

## 🤖 Obstacle Stop Node — How It Works

The `obstacle_stop_node.py` monitors LIDAR data and manages robot safety automatically:

| Parameter | Value |
|-----------|-------|
| **Stop Distance** | 0.5m |
| **Resume Distance** | 0.7m (hysteresis) |
| **Input Topic** | `/cmd_vel_input` (teleop commands) |
| **Output Topic** | `/cmd_vel` (movement commands) |
| **LIDAR Topic** | `/gazebo_ros_laser_controller/out` |
| **Manual Override** | New teleop commands resume movement regardless of obstacles |

**Behavior flow:**
1. Robot moves normally via teleop commands when path is clear
2. If obstacle is detected < 0.5m ahead → robot stops immediately
3. When obstacle moves beyond 0.7m → robot resumes
4. New teleop command → always resumes regardless

---

## 🧠 Key Concepts Learned

- ROS2 node lifecycle and spinning
- Publisher/Subscriber communication pattern
- URDF/Xacro robot modelling
- Gazebo plugins (Differential Drive, LIDAR, Camera)
- Point cloud processing for obstacle detection
- DDS-based communication and QoS policies

---

## 📄 License

This project is for educational purposes. Feel free to use it as a learning reference.
