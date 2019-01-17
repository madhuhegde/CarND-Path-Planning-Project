# CarND-Path-Planning-Project
Self-Driving Car Engineer Nanodegree Program
   
### Goals
The goal of this project is to safely navigate around a virtual highway with other traffic that is driving +-10 MPH of the 50 MPH speed limit. The car's localization and sensor fusion data is provided by the simulator, the sparse map list of waypoints around the highway is available as a file. The car should try to go as close as possible to the 50 MPH speed limit, which means passing slower traffic when possible. The car should avoid hitting other cars at all cost as well as driving inside of the marked road lanes at all times, unless going from one lane to another. The car should be able to make one complete loop around the 6946m highway. Since the car is trying to go 50 MPH, it should take a little over 5 minutes to complete 1 loop. Also the car should not experience total acceleration over 10 m/s^2 and jerk that is greater than 10 m/s^3.

#### The map of the highway is in data/highway_map.txt
Each waypoint in the list contains  [x,y,s,dx,dy] values. x and y are the waypoint's map coordinate position, the s value is the distance along the road to get to that waypoint in meters, the dx and dy values define the unit normal vector pointing outward of the highway loop.

The highway's waypoints loop around so the frenet s value, distance along the road, goes from 0 to 6945.554.



### Data provided from the Simulator to the C++ Program

["x"] The car's x position in map coordinates

["y"] The car's y position in map coordinates

["s"] The car's s position in frenet coordinates

["d"] The car's d position in frenet coordinates

["yaw"] The car's yaw angle in the map

["speed"] The car's speed in MPH


["previous_path_x"] The previous list of x points previously given to the simulator

["previous_path_y"] The previous list of y points previously given to the simulator


["end_path_s"] The previous list's last point's frenet s value

["end_path_d"] The previous list's last point's frenet d value

["sensor_fusion"] A 2d vector of cars and then that car's [car's unique ID, car's x position in map coordinates, car's y position in map coordinates, car's x velocity in m/s, car's y velocity in m/s, car's s position in frenet coordinates, car's d position in frenet coordinates. 

## Details

1. The car uses a perfect controller and will visit every (x,y) point it recieves in the list every .02 seconds. The units for the (x,y) points are in meters and the spacing of the points determines the speed of the car. The vector going from a point to the next point in the list dictates the angle of the car. 

2. There will be some latency between the simulator running and the path planner returning a path, with optimized code usually its not very long maybe just 1-3 time steps. During this delay the simulator will continue using points that it was last given, so oits a good idea to store the last points you have used so you can have a smooth transition. 

## Implementation
I got most of the ideal of implementation from the Project Q&A video.  

 Key steps to get the car drive around successfully are given below
 
1. Use Frenel co-ordinates to plan the trajectory and generate (x,y) co-ordinates using helper functions. The Frenel co-cordinates help to drive the car within the lanes.

2. Gradual increase of velocity to avoid excessive accelaration and jerks

3. Smooth transitions by using left-over points (use last two) and add future co-ordinates from sparse waypoints(30mts, 45mts and 60mts). Then use spline technique to generate (x,y) points and plan the next trajectory

4. The next trajectory is planned for x=26 meters and (x,y) points are generated along the trajectory such that speed limit of 50MPH is not violated. The spline module is used to generate (x,y) points

5. For every 0.02 seconds, 50 point are fed to the simulator - out of 50, the number of newly generated point is (50-prev)


The project also requires that ego car changes lane to pass a slow moving vehicle.  This behavior is implemented in the SW.

1. If a slow moving car is detected in the same lane, start decelaration when the car is within s = 30 meters
2. If there are no cars within s = 22 meters, then change lane
3. If any vehicle is found within 15 meters, avoid collision by higher deceleration