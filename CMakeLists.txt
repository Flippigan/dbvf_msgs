cmake_minimum_required(VERSION 3.8)
project(dbvf_msgs)

find_package(ament_cmake REQUIRED)
find_package(rosidl_default_generators REQUIRED)
find_package(std_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)

rosidl_generate_interfaces(${PROJECT_NAME}
  "msg/LandingTargetPose.msg"
  "msg/TagStatus.msg"
  "msg/VehicleState.msg"
  "srv/SetMode.srv"
  "srv/ArmMotors.srv"
  "srv/SendGuidedPosition.srv"
  "srv/SendGuidedVelocity.srv"
  "srv/StartPrecisionLanding.srv"
  "srv/StartMission.srv"
  "srv/ResumeMission.srv"
  "srv/AbortMission.srv"
  "srv/DoSetServo.srv"
  DEPENDENCIES std_msgs geometry_msgs
)

ament_package()
