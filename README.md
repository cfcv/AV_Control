# ControlAV
This project aims to build a controller to send steering and throttle commands to a simulated self-driving car in order to follow a waypoint trajectory.

## Controller
#### Longitudinal Control
For the longitudinal control a PID was implemented. The proportional coefficients are:
* K_p = 1.908852977263873 
* K_i = 0.6316572093202273
* K_d = 0.0001

Denoting the velocity error as v\_error, it's derivative as dt\_v\_error and it's integral as i\_v\_error the acceleration can be computed as:
```acc = K_p * v_error + K_i * i_v_error + K_d * dt_v_error```

If the result is positive it means an acceleration value and if it is negative it means a brake value. The PID coefficients were selected by the twiddle algorithm. 

## Results

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
