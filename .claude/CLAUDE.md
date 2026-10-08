# CLAUDE.md — dbvf_msgs

## Package Purpose

ROS2 message and service definitions for the DBVF competition autonomy system. Separated from `dbvf_autonomy` because `rosidl_generate_interfaces` and `ament_python_install_package` cannot coexist in the same package with the same name (namespace collision).

## Package Type

Pure `ament_cmake` with `rosidl_default_generators`. No Python code — only .msg and .srv definitions.

## Messages

### LandingTargetPose.msg
```
std_msgs/Header header
int32 tag_id
float64 tag_size
float64 angle_x        # Angular offset from camera center (radians)
float64 angle_y
float64 position_x     # 3D position in body frame (meters)
float64 position_y
float64 position_z
bool position_valid     # False = angles-only mode
```

### TagStatus.msg
```
std_msgs/Header header
bool detected
int32 active_tag_id    # -1 if no tag active
int32 frames_since_last
float64 confidence     # 0.0-1.0, fraction of buffer with active tag
```

### VehicleState.msg
```
std_msgs/Header header
string mode            # ArduPilot mode name (GUIDED, LAND, etc.)
bool armed
float64 lat, lon       # GPS position
float64 alt_rel        # Relative altitude (meters)
float64 vx, vy, vz    # Velocity (m/s)
float64 heading        # Degrees
```

## Services

### SetMode.srv
`string mode` --- `bool success, string message`

### ArmMotors.srv
`bool arm` --- `bool success, string message`

### SendGuidedPosition.srv
`float64 lat, float64 lon, float64 alt` --- `bool success, string message`

### SendGuidedVelocity.srv
`float64 vx, float64 vy, float64 vz` --- `bool success, string message`

### StartPrecisionLanding.srv
`float64 target_lat, float64 target_lon, float64 offset_forward (0.0), float64 offset_right (0.0)` --- `bool success, string message`

### StartMission.srv
`string config_path` --- `bool success, string message`

### ResumeMission.srv
(empty request) --- `bool success, string message`

### AbortMission.srv
`string reason` --- `bool success, string message`

### DoSetServo.srv
`int32 servo_number, int32 pwm` --- `bool success, string message`

### Takeoff.srv
`float64 altitude` --- `bool success, string message`

## Usage

```python
from dbvf_msgs.msg import LandingTargetPose, TagStatus, VehicleState
from dbvf_msgs.srv import SetMode, ArmMotors, SendGuidedPosition, StartPrecisionLanding
from dbvf_msgs.srv import StartMission, ResumeMission, AbortMission, DoSetServo, Takeoff
```

## Build

```bash
colcon build --packages-select dbvf_msgs
```

Must be built before `dbvf_autonomy` (colcon handles ordering via dependency).
