## Table of Contents
1. Content of Package
2. Computational Graph
3. Instruction: How to run the code
4. Robot Behaviour
5. Software Architecture
6. System's Limitation and Possible Improvement

## General Info 
This is the package to simulate the non-holonomic robot 
by using navigation stack on gazebo environment.
The robot is localized by user request.
The robot can implement the following tasks:
1. move randomly in the environment, by choosing 1 out of 6 possible target positions:
   [(-4,-3);(-4,2);(-4,7);(5,-7);(5,-3);(5,1)], implementing a random position service.
2. directly ask the user of the next target position (checking that the position is one of the possible six)
   and reach it
3. start following the external walls
4. stop in the last position

## Content of Package
### Nodes
1. (control)
* ROS publisher: publishing the robot speed
* ROS subscriber: subscribe for robot position
* ROS client: receiving a random target
2. (user_interface)
* ROS server: Service Server replys to the client with a option number
3. (move_random)
* ROS server: Service Server, execute to move robot randomly
* ROS publisher: publishing the robot target position
* ROS subscriber: subscribe for robot position
4. (move_target)
* ROS server: Service Server, executed to move robot to a target
* ROS publisher: publishing the robot target position
* ROS subscriber: subscribe for robot position
5. (random_pos)
* ROS server: Service Server replys to the client with a random target
6. (wall_following)
* ROS server: Service Server, executed to let the robot follow wall
* ROS publisher: publishing the robot speed
* ROS subscriber: subscribe for laser scan value

### Topics
- /cmd_vel
  - Type: gemetry_msgs/Twist
  - Publisher Node: control
  - Subscriber Node: /gazebo 
- /odom
  - Type: nav_msgs/Odometry
  - Publisher Node: /gazebo
  - Subscriber Node: move_random, move_target, /move_base
- /move_base/goal
  - Type: MoveBaseActionGoal
  - Publisher : move_random, move_target, /move_base
  - Subscriber: /move_base
- /move_base/status
  - Type: GoalStatusArray
  - Publisher : /move_base
  - Subscriber: control

### Custom Services
- /move_to_target
  - Client Node: control
  - Server Node: move_to_target
  - Type: SetBool
- /move_random
  - Client Node: control
  - Server Node: move_random
  - Type : SetBool
- /user_interference
  - Client Node: control
  - Server Node: user_interference
  - Type : Empty
- /reading_laser
  - Client Node: control
  - Server Node: wall_follower_switch
  - Type : SetBool
- /random_pos
  - Client Node: control
  - Server Node : random_pos
  - Type : Trigger

## Computational Graph
![Communication Graph](../master/myFolder/rosgraph.png)

## How to run the code
1. Git clone 'final_assignment' and 'slam_gmapping' package
git clone https://github.com/juri-khanmeh/
git clone https://github.com/CarmineD8/slam_gmapping.git
2. Move the repositories to your workspace
3. Build the packages 'catkin_make'
4. Refresh 'rospack profile'
5. Roslaunch 
* roslaunch final_assignment simulation_gmapping.launch
* roslaunch final_assignment move_base.launch
* roslaunch final_assignment final_assignment.launch

## Robot Behaviour
Let's describe the role of each main nodes.

- (control)
  - change the state depending on user's request.

- (move_to_targe)
  - make the robot move to a specific position

- (move_random)
  - let the robot move freely in the environment

- (wall_follow_switch)
  - make the robot follow the external wall

- (user_interface)
  - ask user to enter an option number

- (random_pos)
  -  generate the random target position from [(-4,-3),(-4,2),(-4,7),(5,-7),(5,-3),(5,1)]



## Software Architechture




## System's Limitation and Possible Improvement


