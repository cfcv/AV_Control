# ControlAV


## Setup
The version of CARLA simulator used for this project(0.8.4) only supports python 3.5.x or 3.6.x
1. Download the CARLA simulator binary: 
2. Extract it to the $HOME/opt/ folder
3. Install python dependencies: ```pip install -r $HOME/opt/CarlaSimulator/requirements.txt```
4. Downgrade the matplotlib version by doing: ```pip install matplotlib==3.0.0```
5. check if the server setup is working properly ```.$HOME/opt/CarlaSimulator/CarlaUE4.sh -windowed -carla-no-networking```
6. In the HOME/opt/CarlaSimulator/PythonClient folder run the setup python script: ```python setup.py build``` followed by ```python setup.py install``

This last step will generate a dist folder containing a .egg file. Make sure that the path to the carla folder and the path to the .egg file are in the $PATH environment variable.

# Run

Run the CARLA server:
./CarlaUE4.sh /Game/Maps/RaceTrack -windowed -carla-server -benchmark -fps=30

Run the Client to control the vehicle:
python module_7.py
