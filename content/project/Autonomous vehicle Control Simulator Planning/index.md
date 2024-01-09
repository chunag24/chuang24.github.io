---
title: Simulator Planning Node Breakdown 
summary: A ros node within the control subteam to plan path in simulated environment. 
tags: 
  - Robotics 
  - C++
  - ROS 
date: "2016-04-27T00:00:00Z"
 
---


### 1.Default constructor: SimulatorPlanning()
#### Purpose: Initialize `SimulatorPlanning` node with default settings 
#### Functionality:
- Initialize the `sim_planning_node` 
- Calculate file paths for reading CSV data/
- Declare and initializes parameters for path planning. 
- Set up a publisher for planned paths and a subscriber 
- Read the initial path from a CSV file and publishes the first local path. 
```c++
SimulatorPlanning::SimulatorPlanning()
: Node("sim_planning_node")
{
  std::string file_path = __FILE__;
  std::string dir_path = file_path.substr(0, file_path.rfind("src/sim_planning"));
  std::string file_csv = dir_path + "csv/unit_test_4.csv";
  this->declare_parameter("csv/filename", file_csv);
  this->declare_parameter("path_increment", 0.20);
  this->declare_parameter("t_lookahead", 3.0);
  this->declare_parameter("path_completion_threshold", 0.50);
  this->declare_parameter("min_points", 25);
  filename_ = this->get_parameter("csv/filename").as_string();
  path_increment_ = this->get_parameter("path_increment").as_double();
  t_lookahead_ = this->get_parameter("t_lookahead").as_double();
  path_completion_threshold_ = this->get_parameter("path_completion_threshold").as_double();
  min_points_ = this->get_parameter("min_points").as_int();
  n_lookahead_poses_ = min_points_;

  pub_path_ = this->create_publisher<autoronto_msgs::msg::PlanPath>("/planner/planned_path", 10);
  sub_novatel_inspva_ = this->create_subscription<novatel_oem7_msgs::msg::INSPVA>(
    "/novatel/oem7/inspva",
    10,
    std::bind(&SimulatorPlanning::callback_inspva, this, _1));

  read_csv();
  publish_path(path_idx_, min_points_);   // publish first local path in global_path_
  send_globalpath_srv_to_viz();
  send_globalpath_srv_to_kin();
}
```

### 2.Parameterized Constructor: SimulatorPlanning(std::string f)
#### Purpose: Alternative constructor that allows specifying a CSV file path for path data.
#### Functionality:
- Initializes the node with a specified file path for the CSV.
- Similar to the default constructor, it declares parameters and sets up publishers and subscribers.
- Reads path data from the specified CSV file. 


### 3. `send_globalpath_srv_to_viz()`
#### Purpose: Sends the global path data to a visualization node.
#### Functionality:
- Creates a service client to communicate with a visualization service.
- Prepares a request containing the global path.
- Sends the request and waits for a response, handling any connection issues or service interruptions.
#### Steps:
1. Create a Service Client:

Initializes a ROS service client to communicate with a visualization service. This client is specifically for the `autoronto_msgs::srv::SimGlobalPath` service type.
The service name is `"sim_global_path_viz"`, indicating it's intended for a visualization node that handles global path data.

2. Prepare Service Request:

Constructs a new service request of type `autoronto_msgs::srv::SimGlobalPath::Request`.
Assigns the `global_path_` member variable (which contains the global path data of the vehicle) to the path field of the service request. This is the data that will be sent to the visualization node.

3. Wait for Service Availability:

Enters a loop that waits for the visualization service to become available. This is important to handle situations where the service may not be immediately ready to accept requests. If the ROS node is not operational `(!rclcpp::ok())`, the function returns early, indicating an interruption or shutdown of the ROS node.

4. Send Request to the Service:

Once the service is available, it sends the request to the visualization node.
This is an asynchronous operation, meaning the function will not block waiting for a response. Instead, it proceeds to the next step.

5. Handle Service Response:

Waits for the completion of the service request in a non-blocking manner using `rclcpp::spin_until_future_complete`.
Checks if the service call was successful. If so, it may log or handle the response, which includes confirmation that the path was received by the visualization node.
Handles failure cases by logging an error message. This is crucial for debugging if the service call does not succeed.


```c++
void SimulatorPlanning::send_globalpath_srv_to_viz()
{
  // request service (send viz node the global path)
  rclcpp::Client<autoronto_msgs::srv::SimGlobalPath>::SharedPtr client =
    this->create_client<autoronto_msgs::srv::SimGlobalPath>("sim_global_path_viz");

  auto request = std::make_shared<autoronto_msgs::srv::SimGlobalPath::Request>();
  request->path = global_path_;

  while (!client->wait_for_service()) {
    if (!rclcpp::ok()) {
      RCLCPP_ERROR(this->get_logger(), "Interrupted while waiting for the viz service. Exiting.");
      return;
    }
    RCLCPP_DEBUG(this->get_logger(), "Viz service not available, waiting again...");
  }
  RCLCPP_DEBUG(this->get_logger(), "Viz service is AVAILABLE!");

  auto result = client->async_send_request(request);

  // Wait for the result.
  if (rclcpp::spin_until_future_complete(
      this->get_node_base_interface(),
      result) == rclcpp::FutureReturnCode::SUCCESS)
  {
    RCLCPP_DEBUG(this->get_logger(), "Viz received path: %d", result.get()->path_received);
  } else {
    RCLCPP_ERROR(this->get_logger(), "Failed to call service");
  }
}
```


### 4. `read_csv()`
#### Purpose: Reads path data from a CSV file to construct the global path.
#### Functionality:
- Opens and reads the specified CSV file.
- Parses path points from the file and interpolates additional points as needed.
- Stores the resulting path in a global path variable.
#### Steps
1. Opening the CSV File:

A standard input file stream (std::ifstream) is used to open the CSV file specified by the filename_ member variable.
It checks if the file is successfully opened and logs the status. If the file cannot be opened (due to reasons like file not found, incorrect permissions, etc.), the function logs this and returns early.

2. Reading Data from the File:

The function enters a loop to read the contents of the CSV file.
Inside the loop, it reads individual values corresponding to path points. Typically, these might include coordinates (like `x_utm and y_utm`) and other data such as velocity limits (`v_lim`).
A comma is expected after each data value (since it's a CSV file). The function uses the comma variable to correctly parse the data.

3. Error Handling during File Read:

The function checks for failures in the file stream (using `fs.fail())` or the end of the file (`fs.eof())`. If any of these conditions occur, it breaks out of the loop, indicating the end of the data or an error in reading.

4. Processing and Storing Path Data:

As it reads each set of coordinates and other data, the function performs necessary processing. This might include linear interpolation of the path points based on the path_increment_ parameter.
The processed path points are stored in `global_path_`, which is a vector. This global path represents the entire route that the vehicle will follow.

5. Handling Interpolation:

If interpolation is needed, the function calculates intermediate points between the explicitly defined points in the CSV. This ensures smoother transitions and more accurate path tracking.

6. Closing the File:

After processing all the data, the file stream is closed.

```c++
void SimulatorPlanning::read_csv()
{
  // read in CSV file
  std::ifstream fs;
  fs.open(filename_);

  if (fs.is_open()) {
    RCLCPP_INFO(this->get_logger(), "CSV File open: %s", filename_.c_str());
  } else {
     RCLCPP_INFO(this->get_logger(), "CSV File NOT open: %s", filename_.c_str());
    return;
  }

  uint32_t ctr = 0;
  while (ctr < max_n_poses_) {
    double x_utm, y_utm;
    float v_lim;
    char comma;
    fs >> x_utm >> comma >> y_utm >> comma >> v_lim;
    if (fs.fail() || fs.eof()) {
      break;
    }
    RCLCPP_DEBUG(this->get_logger(), "x_utm: %f, y_utm: %f", x_utm, y_utm);

    // linearly interpolate in path_increment_ steps and push_back all those points.
    // if cannot interpolate because next utm point is less than increment,
    // then discard and dont push back anything!
    autoronto_msgs::msg::PathPoint p;
    p.x = x_utm, p.y = y_utm, p.theta = 0, p.vlim = v_lim;

    if (ctr == 0) {
      global_path_.push_back(p);
    } else {
      append_interpolated_path(p, path_increment_);
    }
    ctr++;
  }

  fs.close();
  RCLCPP_DEBUG(this->get_logger(), "File closed!");

  uint32_t n_global_poses = global_path_.size();
  min_points_ = std::min(n_global_poses, min_points_);
}
```

### 5. `publish_path(int start_idx, int end_idx)`
#### Purpose: To select a segment of the global path (defined from `start_idx` to `end_idx`) and publish it as the current local path for the vehicle to follow.
#### Functionality:
- Opens and reads the specified CSV file.
- Parses path points from the file and interpolates additional points as needed.
- Stores the resulting path in a global path variable.
#### Steps
1. Path Segment Extraction:
The function extracts a subset of the global path, starting from `start_idx` to `end_idx`. This segment represents the immediate path that the vehicle should follow next. The extracted segment is stored in `local_path_`.
2. Checking Path Validity:
The function checks if `start_idx` and `end_idx` are different to ensure that there is a valid path segment to publish.
It calculates the number of poses (`n_pub_poses_`) in the local path by subtracting `start_idx` from `end_idx`.
3. Updating Path Index:
`path_idx_` is updated to `start_idx`. This index might be used to keep track of the current position within the global path.
4. Publishing the Local Path:
If the local path segment is valid and the end of the global path has not been reached (`!last_point_published_`), the function prepares a message of type `autoronto_msgs::msg::PlanPath`.
The local path segment is assigned to the message, and it's published using the `pub_path_` publisher. This publisher communicates the path data to other components or systems that will use it for navigation or further processing.

5. Handling the End of the Path:
The function checks if the end of the global path has been reached (i.e., `end_idx` is the last index of the global path). If so, it sets `last_point_published_` to true. This flag might be used to indicate that the vehicle has reached or is about to reach the end of its planned route.

```c++
void SimulatorPlanning::publish_path(int start_idx, int end_idx)
{
  if (start_idx != end_idx) {
    n_pub_poses_ = end_idx - start_idx;
    path_idx_ = start_idx;
    local_path_ = std::vector<autoronto_msgs::msg::PathPoint>(
      global_path_.begin() + start_idx, global_path_.begin() + end_idx);

    if (!last_point_published_) {
      auto message = autoronto_msgs::msg::PlanPath();
      message.path = local_path_;
      pub_path_->publish(message);
    }

    if ((end_idx == static_cast<int>(global_path_.size() - 1)) && !last_point_published_) {
      last_point_published_ = true;
    }
  }
}
```


### Header FIle 

```c++
namespace controller
{
const rclcpp::Logger DEBUG_LOGGER = rclcpp::get_logger("DEBUG SIM_PLANNING");

class SimulatorPlanning : public rclcpp::Node
{
public:
  SimulatorPlanning();
  explicit SimulatorPlanning(std::string f);

private:
  void send_globalpath_srv_to_viz();
  void send_globalpath_srv_to_kin();
  void read_csv();
  void append_interpolated_path(const autoronto_msgs::msg::PathPoint & p, const float step);
  void publish_path(int start_idx, int end_idx);

  // ros interface
  rclcpp::Publisher<autoronto_msgs::msg::PlanPath>::SharedPtr pub_path_;
  rclcpp::Subscription<novatel_oem7_msgs::msg::INSPVA>::SharedPtr sub_novatel_inspva_;

  // subscriber callbacks
  void callback_inspva(const novatel_oem7_msgs::msg::INSPVA & msg);

  // current vehicle data
  double veh_x_;            // UTM
  double veh_y_;            // UTM
  double veh_velocity_;     // m/s

  const uint32_t max_n_poses_ = 2000U;   // maximum number of rows in the input csv file
  uint32_t n_pub_poses_;                     // actual number of poses in published local path
  uint32_t n_lookahead_poses_;               // desired number of poses in local path
  uint16_t path_idx_ = 0U;                   // global path index of local path's first element
  std::vector<autoronto_msgs::msg::PathPoint> global_path_;
  std::vector<autoronto_msgs::msg::PathPoint> local_path_;
  bool last_point_published_ = false;

  // ros params
  std::string filename_;
  float path_increment_;
  float t_lookahead_;
  float path_completion_threshold_;
  uint32_t min_points_;

  friend class SimulatorPlanningTest;
};
}  // namespace controller

```