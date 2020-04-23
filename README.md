# HOMROS

Restros (or  Restaurant service RObot) is a robot able to perform autonomous task as mapping the enviropment, path planning, moving base, obstacle avoidance..ect. for the main goal of perform a pickup - dropoff task inside a restaurant using ROS.

# Robot mission:
Green box will apear in the restaurant(Rviz Mark) indicating a pickup area (a chitchen). The robot mission is to travel to the pickup area and simulate a pickup operation of 5 sec. after this time, a blue box appears indication the dropoff area (a customer table).
In order to complete the simulation in ROS is needed:
Preparation:
* The robot is deployed in a simulated world.
* The robot uses 3D camera and Scanner to gather information about its sorroundings.
* The robot can use the gathered exploration information to generate a map of the world.
* The robot can navigate inside the world planning its path using the generated map.
Objective:
* The robot recives a pickup mark coordanates, coordanates in the world.
* The robot autonomously travels to the mark using the generated map.
* The robot arrives to the pickup mark and waits for delivery orders.
* The robot recieves a delivery mark coordinates and plans its navigations to it.
* The robot arrives to the delivery mark and gets a : mission acomplished!!.

# Robot Operation Packages:
The robot pickup and delivery process is accomplished by add_markers and pick_objects packages; the firsts sets the marks for the robot to travel to, and the second commands the robot to travel to this marks.

#Pickup Delivery Process:

## The Markers Orders(add_markers package):
Everytime an order is ready for delivery, the markers package generates marks for the robot as:
* The marker should initially be published at the pickup zone. 
* 5 seconds after the robot arrives to the pickup marker, it should be hidden. 
* Then after another 5 seconds it should appear a mark in the drop off zone.
* After robot arrives to the drop off mark, the mark should be hidden.
* Robot should then go back to the wait zone and wait for new markers.
* Proccess continues with a new pickup mark.

Markers packages are launch by the add_marker.sh file that will publish the markers to rviz.
- Marks coordinates: marks dropoff coordinates are recived by the markers package, pickup and waiting zone is always the same. 
- Marks reached: The packages lisen to the robot for mark arrival confirmation , when the robot reach the mark the package can know by itself.
## The Pick Orders (pick_objects):
Command the navigation and path planning for the robot by generating the actual orders to the robot movement.
* The robot stay in a waiting state/ waiting area, while subscribed to DeliveryOrders Channel, and waits for a new order to come.
* The robot sees a New DeliveryOrders Msg and sends request to the service OrdersManager to get this order.
* The robot gets order information if order is adjudicated to the robot.
* The robot moves to the pickup area, shown with a pickup mark and changes state to onduty.
* Ones the robot reach the pickup mark, the robot publish an order update to the OrdersManager service.
* Ones the robot reach the dropoff mark, the robot publish an order update to the OrdersManager service.
* Ones the dropoff is complete, the robot returns to waiting state and lisen for new orders to come.

### Services:     
/add_markers/orders_update
    Server (add_markers.cpp): keep track of the delivery orders states by updating the marks.
    Client (pick_objects.cpp): Use this service to retrieve coordinates each time it accomplished a goal.
    
### Topics:
add_markers/orders:
    Publisher (add_markers.cpp): Publish Markers id, simulating Orders, ready for pickup-dropoff.
    Subscriber(pick_objects.cpp) When robot is waiting or last pickup-dropoff misssion is finish, robot subscribe to get a new delivery Orders. When order/marker id is received, robot unsubscribe from channel. this is intended in case, more than one robot is used. 

## Simulation Setup:
Official ROS packages used:
  gmapping: With the gmapping_demo.launch file, you can easily perform SLAM and build a map of the environment with a robot equipped with laser range finder sensors or RGB-D cameras.
  turtlebot_teleop: With the keyboard_teleop.launch file, you can manually control a robot using keyboard commands.
  turtlebot_rviz_launchers: With the view_navigation.launch file, you can load a preconfigured rviz workspace. You’ll save a lot of time by launching this file, because it will automatically load the robot model, trajectories, and map for you.
  turtlebot_gazebo: With the turtlebot_world.launch you can deploy a turtlebot in a gazebo environment by linking the world file to it.
  Move Robot Package: Robot uses move_base package to set destination coordinates.
Homros Package folders:
  map: Inside this directory, you will store your gazebo world file and the map generated from SLAM.
  scripts: Inside this directory, you’ll store your shell scripts.
  rvizConfig: Inside this directory, you’ll store your customized rviz configuration files.
  pick_objects: You will write a node that commands your robot to drive to the pickup and drop off zones.
  add_markers: You will write a node that model the object with a marker in rviz.

## Localization

## Mapping

## Navigation

### Navigation Goals
Robot navigation goals are set in the script, where multiple goals can be set. Also, turtlebot_teleop package is present, so the robot can be operated manually with the keyboard or similar.


 
# Install and Run:
Run the following code to download, install and run the code
'''
git clone https//:
cd hom
catking_make
./src/scripts/home_service.sh
'''
