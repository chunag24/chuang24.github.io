---
title: Simulator Kinematics Node Breakdown 
summary: A ros node that simulates vehicle kinematics. 
tags:
  - Robotics 
  - C++
  - ROS2

date: "2016-04-27T00:00:00Z"

# Optional external URL for project (replaces project detail page).
 

 
---


### 1.Default constructor: SimKinematics()
#### Subscriptions and publishers:
- Initialize a subscription to a ros topic `"control/command"`, with a message queue size of 50. 
- It subscribes to messages of type `autoronto_msgs::msg::ControllerCommand` and binds the `control_command_callback` to handle incoming messages. 
- Creates publishers for topics related to kinematics simulation output `/SimKinematicsOutput/data` and simulated GPS/IMU data `/novatel/oem7/inspva` and `/novatel/oem7/corrimu` both with a queue size of 50.
#### Service 
- Set ups a service named `sim_global_path_kin` of type `autoronto_msgs::srv::SimGlobalPath` and binds `sim_global_path_callback` to handle service requests.
#### Parameters:
- Decclare various parameters with default values related to controller frequency, simulation frequency, measurement errors, safety factors, and delays. 

#### Timers 
- Sets up timers using `create_wall_timer` to periodically invoke `novatel_inspva_timer_callback` and `novatel_corrimu_timer_callback` callbacks, simulating GPS and IMU data publication.

#### Command Buffer
- Initializes a circular buffer to store control commands, simulating a delay in the command pipeline. 

- Initialize the `sim_planning_node` 
- Calculate file paths for reading CSV data/
- Declare and initializes parameters for path planning. 
- Set up a publisher for planned paths and a subscriber 
- Read the initial path from a CSV file and publishes the first local path. 
```c++
SimKinematics::SimKinematics()
: Node("sim_kinematics_node")
{
  sub_control_cmd_ = this->create_subscription<autoronto_msgs::msg::ControllerCommand>(
    "/control/command", 50,
    std::bind(&SimKinematics::control_command_callback, this, std::placeholders::_1));
  pub_sim_ = this->create_publisher<autoronto_msgs::msg::KinematicsSim>(
    "/SimKinematicsOutput/data",
    50);

  pub_inspva_ =
    this->create_publisher<novatel_oem7_msgs::msg::INSPVA>("/novatel/oem7/inspva", 50);
  pub_corrimu_ =
    this->create_publisher<novatel_oem7_msgs::msg::CORRIMU>("/novatel/oem7/corrimu", 50);

  srv_global_path_ = this->create_service<autoronto_msgs::srv::SimGlobalPath>(
    "sim_global_path_kin", std::bind(
      &SimKinematics::sim_global_path_callback, this, std::placeholders::_1,
      std::placeholders::_2));

  this->declare_parameter("controller_f_hz", 20.0);
  this->declare_parameter("add_noise_", false);
  this->declare_parameter("v_measure_error_stddev_", 0.02);
  this->declare_parameter("pos_measure_error_stddev_", 0.03);
  this->declare_parameter("yaw_measure_error_stddev_", 0.02);
  this->declare_parameter("a_measure_error_stddev_", 0.05);
  this->declare_parameter("sim_f_hz", 100.0);
  this->declare_parameter("novatel_inspva_f_hz", 1.0);
  this->declare_parameter("novatel_corrimu_f_hz", 100.0);
  this->declare_parameter("dt_safety_factor", 1.5);
  this->declare_parameter("time_delay", 0.16);
  this->declare_parameter("imu_data_rate", 200.0);

  controller_dt_ = 1.0 / this->get_parameter("controller_f_hz").as_double();
  add_noise_ = this->get_parameter("add_noise_").as_bool();
  v_measure_error_stddev_ = this->get_parameter("v_measure_error_stddev_").as_double();
  pos_measure_error_stddev_ = this->get_parameter("pos_measure_error_stddev_").as_double();
  yaw_measure_error_stddev_ = this->get_parameter("yaw_measure_error_stddev_").as_double();
  a_measure_error_stddev_ = this->get_parameter("a_measure_error_stddev_").as_double();
  sim_f_hz = this->get_parameter("sim_f_hz").as_double();
  novatel_inspva_f_hz = this->get_parameter("novatel_inspva_f_hz").as_double();
  novatel_corrimu_f_hz = this->get_parameter("novatel_corrimu_f_hz").as_double();
  dt_safety_factor_ = this->get_parameter("dt_safety_factor").as_double();
  time_delay_ = this->get_parameter("time_delay").as_double();
  imu_data_rate_ = this->get_parameter("imu_data_rate").as_double();

  v_measure_noise_ = std::normal_distribution<double>(0.0, v_measure_error_stddev_);
  pos_measure_noise_ = std::normal_distribution<double>(0.0, pos_measure_error_stddev_);
  yaw_measure_noise_ = std::normal_distribution<double>(0.0, yaw_measure_error_stddev_);
  a_measure_noise_ = std::normal_distribution<double>(0.0, a_measure_error_stddev_);

  std::chrono::milliseconds sim_period_ms(1000 / static_cast<int>(sim_f_hz));
  std::chrono::milliseconds novatel_inspva_period_ms(1000 /
    static_cast<int>(novatel_inspva_f_hz));
  std::chrono::milliseconds novatel_corrimu_period_ms(1000 /
    static_cast<int>(novatel_corrimu_f_hz));

  novatel_inspva_timer_ =
    this->create_wall_timer(
    novatel_inspva_period_ms,
    std::bind(&SimKinematics::novatel_inspva_timer_callback, this));
  novatel_corrimu_timer_ =
    this->create_wall_timer(
    novatel_corrimu_period_ms,
    std::bind(&SimKinematics::novatel_corrimu_timer_callback, this));

  buffer_size_ = static_cast<int>(time_delay_ / controller_dt_) + 1;
  cmd_buffer_ = boost::circular_buffer<std::pair<double, double>>(buffer_size_);
  for (int i = 0; i < buffer_size_; i++) {
    cmd_buffer_.push_back(std::make_pair(0.0, 0.0));
  }
}
```

### 2. `Control_command_callback`
#### Purpose: Handles incoming control commands by storing them in the buffer and simulating vehicle dynamics if a global path has been received.
#### Functionality:
1. This callback function is designed to respond to incoming messages on the `/control/command` topic, which are of type `autoronto_msgs::msg::ControllerCommand`. `msg` is a shared pointer to the `controllercommand` message received from the topic. 
2. Command BUffering 
  - If the global path has been received, it pushes the torque, and steering wheel angle from the incoming message onto the `cmd_buffer`. This circular buffer represents a time delay in the command processiong, to simulate real world behavior where commands are not executed immediately. 
3. Command execution
  - It then takes the oldest command, at index 0, representing the delayed execution of control commands.
4. Simulation Update
  - After updating the control commands, the function then calls `sim_run()` where the simulation for vehicle's kinematic is computed. `sim_run` will use the current `torque_` and `rwa_` to update vehicle's states in the simulation, which includes position, velocity, and acceleration based on the bicycle model. 
```c++
void SimKinematics::control_command_callback(const autoronto_msgs::msg::ControllerCommand::SharedPtr msg){
  if (path_received_ == true){
    cmd_buffer_.push_back(std::make_pair(msg->torque, msg->rwa));
    torque_ = cmd_buffer_.at(0).first;
    rwa_ = cmd_buffer_.at(0).second; 
    sim_run();
  }
}
```


### 3. `sim_global_path_callback()`
#### Purpose: Receives a global path request, sets the initial orientation of the vehicle based on the path, and marks the global path as received.
#### Functionality:
- Creates a service client to communicate with a visualization service.
- Prepares a request containing the global path.
- Sends the request and waits for a response, handling any connection issues or service interruptions.
#### Steps:
1. Global Paht Assignment:
- The function begins by assigning the `global_path_` member variable with the path received from the service request `request->path`. This global path contains waypoints that the vehicle should follow.
2. Initial Orientation Calculation
- It first calculates the initial orientation of the vehicle based on the drection between the first 2 waypoints. 
3. Service Response and Flag update 
- After receiving and processing the global path, the function sets the `path_received` to true, 



```c++
void SimKinmeatics::sim_global_path_callback(const std::shared_ptr<autoronto_msgs::srv::SimGlobalPath::Request> request,
std::shared_ptr<autoronto_msgs::srv::SimGlobalPath::Response> response){
  global_path_ = request->path;
  //set vehicle's initial orientation in direction of path
  theta_ = atan2(global_path_[1].y - global_path_[0].y, global_path_[1].x - global_path_[0].x,);
  response->path_recevied = true; 
  path_received_ = true; 
  RCLCPP_INFO(this->get_looger(), "Global path received! theta_ : %f", theta_);
}
```

### 4. `novatel_inspva_timer_callback`
#### Purpose: Periodically simulates and publishes GPS data (latitude, longitude, azimuth, and velocities) on the `"/novatel/oem7/inspva"` topic.
#### Functionality:
This is a timer callback function, designed to be called at regular intervals defined by a timer. It is to simulate the periodic publication of INSPVA messages. 

 
#### Steps
1. Path check 

The function first checks if a global path has been received, as it relies on the path data to simulate vehicle's position and movment. 

2. Simulate poistion 

If the global path is available, the function proceeds to simulate the vehicle's current UTM position. It adds the simulated x and y displacements (x_ and y_) to the starting point of the global path (global_path_[0].x and global_path_[0].y) to calculate the current easting and northing of the UTM point. It also sets the altitude, zone, and band of the UTM point, which are required for a complete UTM specification.

3. Conversion to geographic coordinates: 

The UTM coordinates are converted to geographic latitude and longitude using a function,  provided by a geodesy library (indicated by `geodesy::toMsg(utm_pt)`).

4. Simulate INSPVA message 

An `INSPVA` message is created and populated with the simulated geographic position (latitude, and longitude).

5. Simulate Azimuth:

The function calculates the vehicle's azimuth (its orientation relative to the north). It converts the simulated heading theta_ from radians to degrees and adjusts it to the correct reference frame. Azimuth is usually measured clockwise from North, while theta_ might be counterclockwise from East, hence the transformation.

6. Simulate Velocities 

The eastward and northward velocities are calculated based on the vehicle's simulated velocity (v_) and heading (theta_). This involves a conversion from the vehicle's local reference frame to the geographic reference frame. The simulated INSPVA message is published on the "/novatel/oem7/inspva" topic, which other components of the system can subscribe to for position, velocity, and attitude data.

```c++
void SimKinematics::novatel_inspva_timer_callback()
{
  if (path_received_) {
    geodesy::UTMPoint utm_pt;
    utm_pt.easting = x_ + global_path_[0].x;
    utm_pt.northing = y_ + global_path_[0].y;
    utm_pt.altitude = 0.0;
    utm_pt.zone = 17;
    utm_pt.band = 'T';
    geographic_msgs::msg::GeoPoint pt = geodesy::toMsg(utm_pt);

    auto inspva_msg = novatel_oem7_msgs::msg::INSPVA();
    inspva_msg.latitude = pt.latitude;
    inspva_msg.longitude = pt.longitude;

    double azimuth;
    azimuth = (-theta_ + M_PI_2) * (180.0 / M_PI);
    azimuth = (azimuth < 0) ? (azimuth + 360) : azimuth;
    inspva_msg.azimuth = azimuth;
    inspva_msg.east_velocity = v_ * cos(theta_);
    inspva_msg.north_velocity = v_ * sin(theta_);

    pub_inspva_->publish(inspva_msg);
  }
```

### 5. `novatel_corrimu_timer_callback`
#### Purpose: Periodically simulates and publishes IMU data (corrected inertial measurement unit data) on the `"/novatel/oem7/corrimu"`topic.
#### Functionality:
This function is specifically designed to simulate the periodic publication of CORRIMU (Corrected Inertial Measurement Unit) messages, which are typical outputs from an IMU sensor, like those produced by NovAtel systems
#### Steps
1. Path Check
2. Create CORRIMU message 
- The function constructs a new CORRIMU message which will hold the simulated IMU data. This message type typically includes information such as acceleration and angular rates.
3. Populate CORRIMU Message 
- The corrimu_msg is populated with IMU data. Specifically, the longitudinal_acc field is assigned the simulated longitudinal acceleration. This value is calculated by taking the current simulation's longitudinal acceleration (a_long_) and adjusting it according to the IMU data rate (imu_data_rate_) and the count of IMU data points (corrimu_msg.imu_data_count). This calculation reflects how the raw acceleration data would be integrated over a period to give the corrected acceleration value.
4. Publish CORRIMU Message 
The populated CORRIMU message is then published to the "/novatel/oem7/corrimu" topic. This allows other components or nodes within the ROS ecosystem that require IMU data to receive the simulated information as if it were from an actual NovAtel CORRIMU sensor.


```c++
void SimKinematics::novatel_corrimu_timer_callback()
{
  auto corrimu_msg = novatel_oem7_msgs::msg::CORRIMU();
  corrimu_msg.imu_data_count = 100;
  corrimu_msg.longitudinal_acc = a_long_ / imu_data_rate_ * corrimu_msg.imu_data_count;
  pub_corrimu_->publish(corrimu_msg);
}
```

### 6. `sim_run()`
#### Purpose: Runs the simulation step using the simple bicycle model, updates vehicle's kinematic state, and publishes the simulated data to `"/SimKinematicsOutput/data"` topic. This function is called typically after new control commands have been received and buffered. 
#### Steps:
1. Kinematic Model Update 
- The function first invokes `simply_bicycle(torque_, rwa_)`, which implements the bicycle model of vehicle dynamics. The torque and steering wheel angel are used to calculate the next state, including position, orientaion, velocity.
2. Prepare output message 
- An output message of type `autoronto_msgs::msg::KinematicsSim` is created to hold the updated state of the vehicle. 
- The message is populated with various kinematic properties calculated from the `simple_bicycle` method. 
3. Publish output message 
The populated message is then published to the `/SimKinematicsOutput/data` topic using the `pub_sim` publisher. This makes the updated kinematic state available to all other nodes that are subscribing to this topic.
 



### 7. `simple_bicycle()`
#### Purpose: A kinematic model of the vehicle motion. It takes torque and steering wheel angle as inputs and updates the vehicle's states, includign position, velocity, accel, and orientation. 



