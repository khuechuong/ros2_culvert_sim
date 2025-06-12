# ros2_culvert_sim

## Dependencies
- [turtlebot3_simulations](https://github.com/ROBOTIS-GIT/turtlebot3_simulations) (humble branch NOT humble-devel)
- [velodyne_simulator](https://bitbucket.org/DataspeedInc/velodyne_simulator/src/ros2/) (```sudo apt install```)

## Modifications
### Turtlebot3
I choose waffle_pi so put ```export TURTLEBOT3_MODEL=waffle_pi``` in ```.bashrc```
#### Velodyne
- Inside ```~/turtlebot3_simulations/turtlebot3_gazebo/models/turtlebot3_waffle_pi/model.sdf``` add in

```xml
<link name="velodyne">    
      <inertial>
        <pose>-0.052 0 0.111 0 0 0</pose>
        <inertia>
          <ixx>1e-7</ixx>
          <ixy>0.000</ixy>
          <ixz>0.000</ixz>
          <iyy>1e-7</iyy>
          <iyz>0.000</iyz>
          <izz>1e-7</izz> 
        </inertia>
        <mass>0.114</mass>
      </inertial>

      <collision name="velodyne_sensor_collision">
        <pose>-0.052 0 0.111 0 0 0</pose>
        <geometry>
          <cylinder>
            <radius>0.0508</radius>
            <length>0.055</length>
          </cylinder>
        </geometry>
      </collision>

      <visual name="velodyne_sensor_visual">
        <pose>-0.064 0 0.121 0 0 0</pose>
        <geometry>
          <mesh>
	          <uri>package://velodyne_description/meshes/VLP16_scan.dae</uri>
            <scale>0.001 0.001 0.001</scale>
          </mesh>
        </geometry>
      </visual>

      <sensor name="velodyne-VLP16" type="ray">
        <always_on>true</always_on>
        <visualize>false</visualize>
        <pose>-0.064 0 0.121 0 0 0</pose>
        <update_rate>10</update_rate>
        <ray>
          <scan>
            <horizontal>
              <samples>512</samples>
              <resolution>1.000000</resolution>
	            <min_angle>-3.141592653589793</min_angle>
              <max_angle>3.141592653589793</max_angle> 
            </horizontal>
	          <vertical>
              <samples>64</samples>
              <resolution>1</resolution>
              <!-- 45 degrees -->
              <min_angle>-0.3926991</min_angle>
              <max_angle> 0.3926991</max_angle>
            </vertical>
          </scan>
          <range>
            <min>0.3</min>
            <max>10.0</max>
            <resolution>0.001</resolution>
          </range>
          <noise>
            <type>gaussian</type>
            <mean>0.0</mean>
            <stddev>0.00</stddev>
          </noise>
        </ray>
	      <plugin filename="libgazebo_ros_velodyne_laser.so" name="gazebo_ros_laser_controller">
          <ros>
            <namespace>/velodyne</namespace>
	          <remapping>~/out:=/velodyne_points</remapping>
          </ros>
          <output_type>sensor_msgs/PointCloud2</output_type>
          <frame_name>velodyne</frame_name>
	        <organize_cloud>False</organize_cloud>
          <min_range>0.3</min_range>
          <max_range>10.0</max_range>
          <gaussian_noise>0.008</gaussian_noise>
        </plugin> 
      </sensor>
    </link>
```

- Inside ```~/turtlebot3_simulations/turtlebot3_gazebo/urdf/turtlebot3_waffle_pi.urdf```, add in

```xml
  <joint name="velodyne_joint" type="fixed">
    <origin xyz="-0.064 0 0.121" rpy="0 0 0"/>
    <parent link="base_link"/>
    <child link="velodyne"/>
  </joint>
  <link name="velodyne"/>
```
#### Multiple RGB Cams
