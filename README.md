# ControlAV
This project aims to build a controller to send steering and throttle commands to a simulated self-driving car in order to follow a waypoint trajectory. Click in the image to check out the video.

<p align="center">
<a href="https://www.youtube.com/watch?v=2qNGwuyDDZ0">
<img src="http://i3.ytimg.com/vi/2qNGwuyDDZ0/hqdefault.jpg">
</a>
</p>

## Results

<p align="center">
<img src="https://github.com/cfcv/ControlAV/blob/master/controller_output/speed_profile.png">
</p>

<p align="center">
<img src="https://github.com/cfcv/ControlAV/blob/master/controller_output/trajectory.png">
<img src="https://github.com/cfcv/ControlAV/blob/master/controller_output/waypoints.png">
</p>

## Controller Implementation
#### Longitudinal Control
For the longitudinal control a PID was implemented. The proportional, derivative and integral coefficients are:
* K_p = 1.908852977263873 
* K_i = 0.6316572093202273
* K_d = 0.0001

Denoting the velocity error as v\_error, it's derivative as dt\_v\_error and it's integral as i\_v\_error the acceleration can be computed as:<br/>
```acc = K_p * v_error + K_i * i_v_error + K_d * dt_v_error```

If the result is positive it means an acceleration value and if it is negative it means a brake value. The PID coefficients were selected by the twiddle algorithm. 

#### Lateral Control
For the lateral control a stanley controller was implemented, taking into account the heading and crosstrack error related to the current waypoint trajectory.
1. Heading error<br/>

The current yaw of the vehicle is provided by the measurements, so we need to calculate just the yaw of the trajectory. This is simply the angle formed by the desired waypoint trajectory and can be computed as follows:<br/>
```yaw_trajectory = np.arctan2(waypoints[-1][1] - waypoints[0][1], waypoints[-1][0] - waypoints[0][0])```

Once we have the yaw of the vehicle and the yaw of the trajectory we can compute the heading error by doing:
```heading_error = yaw_trajectory - yaw_car```

2. Crosstrack error<br/>

The crosstrack is the closest distance from the vehicle's current position and the trajectory. The crosstrack error term for the steering angle is the angle between the distane to the trajectory and the velocity vector. This can be computed as follows:<br/>
```crosstrack_error_term = np.arctan2(k_e * crosstrack_error, velocity)```

Where k_e is a cofficient that multiplies the distance to the desired trajectory. It was manually selected ans setted to k_e = 0.8 

3. Putting all together<br/>

```Steer = heading_error + crosstrack_error_term```

## Setup
The version of CARLA simulator used for this project(0.8.4) only supports python 3.5.x or 3.6.x
1. Download the CARLA simulator binary: 
2. Extract it to the $HOME/opt/ folder
3. Install python dependencies: ```pip install -r $HOME/opt/CarlaSimulator/requirements.txt```
4. Downgrade the matplotlib version by doing: ```pip install matplotlib==3.0.0```
5. check if the server setup is working properly ```./$HOME/opt/CarlaSimulator/CarlaUE4.sh -windowed -carla-no-networking```
6. In the $HOME/opt/CarlaSimulator/PythonClient folder run the setup python script: ```python setup.py build``` followed by ```python setup.py install```

This last step will generate a dist folder containing a .egg file. Make sure that the path to the carla folder and the path to the .egg file are in the $PATH environment variable.

7. Clone this repository inside the $HOME/opt/CarlaSimulator/PythonClient

## Run
In order to run the project you have to first run the server side executing the command:

```./$HOME/opt/CarlaSimulator/CarlaUE4.sh /Game/Maps/RaceTrack -windowed -carla-server -benchmark -fps=30```

Finally you can run the client side that will send data to control the vehicle

```python $HOME/opt/CarlaSimulator/PythonClient/ControlAV/module_7.py```

## Conclusion
This simple contoller can successifully follow a desired trajectory and a target speed within this route simulation, here we don't take into account dynamic objects that could interact with the vehicle or the urban rules as limit speed and traffic lights. This projects just aims to output controls to follow a desired trajectory. One improvement that could be done is to use Model Predictive Control, in that way we could add contraints as maximum jerk. 
