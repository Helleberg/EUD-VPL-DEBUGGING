# README

## Quick start

### 1. Download the project

```bash
# Clone or download the project
cd path/to/mj_pick_and_place
```

### 2. Create virtual environment

> Note: The virtual environment needs to be of Python version 3.12

**Linux/Mac:**
```bash
python3 -m venv venv
source venv/bin/activate
```

**Windows:**
```bash
python -m venv venv
venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

## How to use the API

### 1. Run main.py from the root folder
**Windows:**
```bash
python main.py
```

### 2. Open API docs
Open docs through this link: [http://localhost:8000/docs](http://localhost:8000/docs)

### Extra

Missions JSON schema:
```JSON
{
  "missions": [
    {
      "mission_id": "string",
      "actions": [
        {
          "action": "move_to_position",
          "position": {
            "x": 0,
            "y": 0,
            "z": 0
          },
          "duration": 2
        },
        {
          "action": "move_to_object",
          "object_name": "string",
          "height_offset": 0.05
        },
        {
          "action": "pick_object",
          "object_name": "string"
        },
        {
          "action": "grasp"
        },
        {
          "action": "release"
        },
        {
          "action": "wait",
          "seconds": 0
        },
        {
          "action": "initialize"
        },
        {
          "action": "reset_boxes"
        }
      ]
    }
  ]
}
```


## Integration guide of robot_interface/robot_api.py

### Overview

This guide explains how to integrate your EUP tool with the Mujoco robot simulation. 
You can create any interface you want (web-based, desktop app, visual programming, DSL, etc...) as long as it can call Python functions.

### What you get
This project provides:
- **Mujoco simulation**: A physics simulation of a UR5e robot arm and a workstation
- **Robot API**: A simple python interface to control the robot from your EUP tool 

### What you need to build
Your EUP tool that:
1) Includes a user-friendly interface for programming robot tasks
2) Maps user actions into Robot API function calls
3) Manages the simulation loop to continuously keep track and execute commands
---

### Important: a simulation loop is needed !

MuJoCo requires a continuous loop that advances physics at regular intervals. 

```python
while running:
    robot.step_simulation()  #Advance physics by one timestep
    time.sleep(robot.model.opt.timestep)  #0.005 seconds usually
```

### How movement works

```python
#This doesn't move the robot:
robot.move_to_position(0.3, 0.3, 0.6)
#It creates a trajectory an returns immediately

#For execution : this is how movement happens :
while len(robot.current_trajectory) > 0:
    robot.step_simulation()#Execute one waypoint
    time.sleep(robot.model.opt.timestep)#Wait 5ms
```
---

### How to use the Robot API reference ?

```python
from robot_interface.robot_api import robot
import time
```

