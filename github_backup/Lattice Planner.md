This is the current F1Tenth Implementation:
### **`car.cpp`**
```
#include "car.hpp"

Car::Car() : x(0), y(0), z(0), theta(0), q(0, 0, 0, 1), 
             forward_vector(1, 0, 0), 
             lateral_vector(0, 1, 0), 
             vertical_vector(0, 0, 1) {}

void Car::update_values(const geometry_msgs::msg::Pose car_pose){
    x = car_pose.position.x;
    y = car_pose.position.y;
    z = car_pose.position.z;

    q.setValue(car_pose.orientation.x, car_pose.orientation.y, car_pose.orientation.z, car_pose.orientation.w);

    update_unit_vectors(q);
    update_theta();
}

void Car::update_unit_vectors(const tf2::Quaternion& q){
    tf2::Matrix3x3 rotation_matrix(q);
  
    forward_vector = rotation_matrix.getColumn(0);
    lateral_vector = rotation_matrix.getColumn(1);
    vertical_vector = rotation_matrix.getColumn(2);
}


void Car::update_theta() {
    tf2::Vector3 x_unit(1, 0, 0);  // Reference X-axis vector

    if (forward_vector.length() > 0) {
        forward_vector.normalize();
    }
    double angle = std::atan2(forward_vector.y(), forward_vector.x());

    theta = angle;
}
```

### **`curve_gen.cpp`**
```
#include "curve_gen.hpp"

Lattice::Lattice(Map in_map, double DIST_PER_STEP ,int VERT_PER_STEP, int RES_PER_STEP, double BUFFER, int& SAMPLE_SIZE){

    map = in_map;
    resolution = RES_PER_STEP;

    generate_vertices(DIST_PER_STEP, VERT_PER_STEP, BUFFER, SAMPLE_SIZE);

}

int Lattice::find_closest_vertices_idx(Car ego_car){

    Point pose = Point(ego_car.get_x(), ego_car.get_y(), ego_car.get_theta(), 0.0);
    Midpoint base = map.get_closest_midline(pose, step);

    double dist;
    double min_dist = 1e16;
    int min_idx = -1;

    for (size_t i = 0; i < vertice_group.size(); i++) {
        Point vertex = vertice_group[i][2]; //get middle point

        dist = euc_dist(base.x,vertex.x,base.y,vertex.y);
        if(dist < min_dist){
            min_dist = dist;
            min_idx = i;
        }
    }

    if(min_dist>-1){

        //check if point is in right direction
        double rejection_range = 0.9; //compared to pi
        double delta_angle = abs(std::atan2( (vertice_group[min_idx][2].y - base.y), (vertice_group[min_idx][2].x - base.x) )) - base.theta;
        if(delta_angle > rejection_range*M_PI && delta_angle < M_PI/rejection_range){
            if(min_idx + 1 < vertice_group.size()){
                return min_idx+1;
            }
            else{
                return 0;
            }
        }

        return min_idx;
    }
    else{
        std::cerr << "closest vertice group could not be found" << std::endl;
        return -1;
    }

}

std::vector<std::vector<Point>> Lattice::getTrajectories(Car ego_car, bool on_raceline, int current_idx){

    std::vector<std::vector<Point>> traj;
    Point start;
    std::vector<Point> target_set;

    int starting_idx = find_closest_vertices_idx(ego_car);

    if(starting_idx != prev_curve_idx){

        start = on_raceline ? raceline_vertices[starting_idx] : vertice_group[starting_idx][current_idx];

        if(starting_idx+1 == vertice_group.size()){
            target_set = vertice_group[0];
        }
        else{
            target_set = vertice_group[starting_idx+1];
        }

        for(const auto& target : target_set){
            std::vector<Point> points;
            generateCurve(start, target, resolution, points);
            traj.push_back(points);
        }

        if(on_raceline){
            std::vector<Point> points;

            int end_idx = starting_idx+1 == vertice_group.size() ? 0 : starting_idx+1;

            for(int i = raceline_vert_idx.at(starting_idx); i < raceline_vert_idx.at(end_idx); i++){
                points.push_back(map.get_raceline(i));
            }

            traj.push_back(points);

        }

        prev_curve_idx = starting_idx;
    }
    return traj;
}

void Lattice::generate_vertices(double DIST_PER_STEP ,int VERT_PER_STEP, double BUFFER, int& SAMPLE_SIZE)
{
  
  double VERTEX_X_OFFSET;

  int midline_size = map.get_midline_size();

  SAMPLE_SIZE = 0;
 
  for(int i = 0; i < midline_size; i++){

    static double local_dist = 0;
    static Midpoint curr_point = map.get_midpoint(0);
    static Midpoint next_point = map.get_midpoint(1);

    if(i != 0){
        curr_point = next_point;
        next_point = i+1 < midline_size ? map.get_midpoint(i+1) : map.get_midpoint(0);
    }

    if(local_dist < DIST_PER_STEP){
        local_dist += euc_dist(curr_point.x, next_point.x, curr_point.y, next_point.y);
    }
    else{
        std::vector<Point> vertices;

        for(int j = 0; j < VERT_PER_STEP; j++){

            if(j <= VERT_PER_STEP/2){
              VERTEX_X_OFFSET = (curr_point.w_i - BUFFER) / (VERT_PER_STEP/2);
            }
            else{
              VERTEX_X_OFFSET = (curr_point.w_o - BUFFER) / (VERT_PER_STEP/2);
            }
      
            double x = curr_point.x + (VERTEX_X_OFFSET * (j - VERT_PER_STEP/2) * std::cos(curr_point.theta + M_PI_2));
            double y = curr_point.y + (VERTEX_X_OFFSET * (j - VERT_PER_STEP/2) * std::sin(curr_point.theta + M_PI_2));
            double theta = curr_point.theta;
            double kappa = (curr_point.kappa > 1e-6) ? (1 / ((1 / curr_point.kappa) + VERTEX_X_OFFSET)) : 0.0;
      
            Point vertex = {x,y,theta,kappa};
            vertices.push_back(vertex);
        }

        local_dist = 0;
        SAMPLE_SIZE++;

        int raceline_idx;

        raceline_vertices.push_back(map.get_associated_raceline(vertices, raceline_idx));
        raceline_vert_idx.push_back(raceline_idx);
        vertice_group.push_back(vertices);
    }

  }

}

// Calculate cubic spiral coefficients
void Lattice::calculateSpiralCoeffs(double p0, double p1, double p2, double p3, double sf,
                           double& a, double& b, double& c, double& d) {
    a = p0;
    b = (-11 * p0 + 18 * p1 - 9 * p2 + 2 * p3) / (2 * sf);
    c = (9 * (2 * p0 - 5 * p1 + 4 * p2 - p3)) / (2 * sf * sf);
    d = (-9 * (p0 - 3 * p1 + 3 * p2 - p3)) / (2 * sf * sf * sf);
}


// Generate spiral and return final state
Point Lattice::generateSpiral(Point start, double a, double b, double c, double d, double sf, int steps, std::vector<Point>& points) {
    double ds = sf / steps;
    double x = start.x, y = start.y, theta = start.theta, accel = start.accel;
    double vel, time;

    for (int i = 1; i <= steps; ++i) {
        double s = i * ds;
        double kappa = a + b * s + c * s * s + d * s * s * s;
        theta += kappa * ds;
        x += cos(theta) * ds;
        y += sin(theta) * ds;
        vel = (start.vel + 2*accel*s) >= 0 ? std::sqrt(start.vel + 2*accel*s) : -1;
        time = accel == 0 ? sf/vel : (vel - start.vel)/accel;

        Point point = {x,y,theta,kappa,vel,accel,time};
        points.push_back(point);
    }

    return {x, y, theta, d};
}

// Compute the error between actual and target endpoints
Eigen::Vector4d Lattice::computeError(const Point& actual, const Point& target) {
    Eigen::Vector4d error;
    error << actual.x - target.x,
             actual.y - target.y,
             actual.theta - target.theta,
             actual.kappa - target.kappa;
    return error;
}

// Compute the Jacobian using finite differences
Eigen::Matrix4d Lattice::computeJacobian(const Point& start, const Point& target, Eigen::Vector3d params, int steps, double h) {
    Eigen::Matrix4d J;

    std::vector<Point> final_plus_points;
    std::vector<Point> final_minus_points;

    for (int i = 0; i < 3; ++i) {
        Eigen::Vector3d params_plus = params;
        Eigen::Vector3d params_minus = params;

        params_plus[i] += h;
        params_minus[i] -= h;

        double a, b, c, d;
        calculateSpiralCoeffs(start.kappa, params_plus[0], params_plus[1], target.kappa, params_plus[2], a, b, c, d);
        Point final_plus = generateSpiral(start, a, b, c, d, params_plus[2], steps, final_plus_points);

        calculateSpiralCoeffs(start.kappa, params_minus[0], params_minus[1], target.kappa, params_minus[2], a, b, c, d);
        Point final_minus = generateSpiral(start, a, b, c, d, params_minus[2], steps, final_minus_points);

        Eigen::Vector4d error_plus = computeError(final_plus, target);
        Eigen::Vector4d error_minus = computeError(final_minus, target);

        Eigen::Vector4d derivative = (error_plus - error_minus) / (2 * h);
        J.col(i) = derivative;
    }
    return J;
}

// Iteratively refine curve using Newton's method
void Lattice::generateCurve(Point start, Point target, int steps, std::vector<Point>& points) {
    double p1 = (start.kappa + target.kappa)/2;
    double p2 = p1;
    double s = euc_dist(start.x, target.x, start.y, target.y);

    Eigen::Vector3d params(p1, p2, s);
    
    const int maxIterations = 20;
    const double tolerance = 0.25; //cm
    double norm;

    for (int iter = 0; iter < maxIterations; ++iter) {
        points.clear();

        double a, b, c, d;
        calculateSpiralCoeffs(start.kappa, params[0], params[1], target.kappa, params[2], a, b, c, d);
        Point final_state = generateSpiral(start, a, b, c, d, params[2], steps, points);

        Eigen::Vector4d error = computeError(final_state, target);
        norm = error.norm();

        std::cout << "Iteration " << iter << " | Error norm: " << norm << std::endl;

        if (norm < tolerance) break;

        Eigen::Matrix4d J = computeJacobian(start, target, params, steps);
        Eigen::Vector3d delta = J.topLeftCorner(3, 3).inverse() * error.head(3);

        params -= delta;
    }
    
    if (norm > tolerance){ 
        //if the paths are not converging, try repeat the process with Damping Factor (Levenberg–Marquardt Style)
        std::cerr << "Newton's method did not converge after 20 iterations. Final error: " << norm << "\n";
    }

}

// int main() {
//     Point start = {0.0, 0.0, 0.0, 0.0};
//     Point target = {10.0, 2.0, 0.5, 0.1};

//     Eigen::Vector3d params(0.05, 0.05, 10.0); // Initial guess: p1, p2, s_f

//     int steps = 100;
//     refineCurve(start, target, params, steps);

//     std::cout << "\nFinal Parameters: p1 = " << params[0] << ", p2 = " << params[1] << ", s_f = " << params[2] << std::endl;
//     return 0;
// }
```

### **`map.cpp`**
```
#include "map.hpp"

Map::Map(std::string midline_path, std::string raceline_path, double& MAX_KAPPA) : m_size(0), r_size(0) {
    m_path = midline_path;
    r_path = raceline_path;

    if(!generate_raceline(MAX_KAPPA)){
        file_found = false;
    }

    if(generate_midline()){
        file_found = true;
        generate_angles_and_curvatures();
    }
    
}

bool Map::generate_raceline(double& MAX_KAPPA){
    /*
    Opens raceline csv and extracts x, y, theta, kappa, velocity, acceleration, and station. 
    station gets discard while the rest is stored within a Point. Check common.hpp for Point
    definition. All points are stored in raceline vector. 
    NOTE: theta gets normalised to be within -pi and pi
    Returns boolean value to confirm if opening and extracting data was successful
    */
    MAX_KAPPA = -1e6;

    std::ifstream mapData(r_path);
    if (!mapData.is_open()) {
        return false;
    }

    std::string line;
    double x_m, y_m, theta, kappa, vel, accel, station;
    int file_size = 0;

    // Skip first three line (format comment)
    std::getline(mapData, line);
    std::getline(mapData, line);
    std::getline(mapData, line);

    while (std::getline(mapData, line)) {

        for(size_t i = 0; i<line.length(); i++){
          if(line.at(i) == ';'){
            line.replace(i,1,1,' ');
          }
        }
  
        std::stringstream numStream(line);

        numStream >> station >> x_m >> y_m >> theta >> kappa >> vel >> accel;

        Point data = {x_m, y_m, normalise_angle(theta), kappa, vel, accel};

        if(fabs(kappa) > MAX_KAPPA) MAX_KAPPA = fabs(kappa);

        raceline.push_back(data);
        file_size++;
    }


    if(file_size > 1){
        r_size = file_size;
        return true;
    }

    mapData.close();
    return false;

}

bool Map::generate_midline(){
    /*
    Opens midline csv file and extracts, x, y, inner width(w_i), and outer widht(w_o) for each point. 
    All data is stored in a Point data structure. Check common.hpp for more information. Angle and Curvature are generated 
    in generate_angles_and_curvatures()
    Returns bool for if opening and extracting the data was a success
    */

    std::ifstream mapData(m_path);
    if (!mapData.is_open()) {
        return false;
    }

    std::string line;
    double x_m, y_m, w_i, w_o;
    int file_size = 0;

    // Skip first line (format comment)
    std::getline(mapData, line);

    while (std::getline(mapData, line)) {
  
        for(size_t i = 0; i<line.length(); i++){
          if(line.at(i) == ','){
            line.replace(i,1,1,' ');
          }
        }
  
        std::stringstream numStream(line);

        numStream >> x_m >> y_m >> w_i >> w_o;
  
        //theta and kappa will be updated later
        Midpoint data = {x_m, y_m, 0,0, w_i, w_o};

        midline.push_back(data);
        file_size++;
    }


    if(file_size > 1){
        m_size = file_size;
        return true;
    }

    mapData.close();
    return false;
}

void Map::generate_angles_and_curvatures(){
    /*
    Angles are generated using the x and y components of a vector formed from the current point
    to the next point. Curvature is generated as an integral of angle from 0 to ending station,
    and calculated using riemann sum. 
    */

    for(int i = 0; i<m_size-1; i++){

        double angle, dx, dy;

        if(i == m_size-1){
            dx = midline[0].x - midline[i].x;
            dy = midline[0].y - midline[i].y;     
        }
        else{
            dx = midline[i+1].x- midline[i].x;
            dy = midline[i+1].y - midline[i].y;
        }

        angle = std::atan2(dy,dx);

        midline[i].theta = angle;
    }

    for(int i = 0; i<m_size-1; i++){

        double curvature, dx, dy, ds, dtheta;

        if(i == m_size-1){
            dx = midline[0].x - midline[i].x;
            dy = midline[0].y - midline[i].y;

            dtheta = midline[0].theta - midline[i].theta; // Angle difference        
        }
        else{
            dx = midline[i+1].x- midline[i].x;
            dy = midline[i+1].y - midline[i].y;

            dtheta = midline[i+1].theta - midline[i].theta; // Angle difference
        }

        ds = std::sqrt(dx * dx + dy * dy);  // Step size (arc length difference), computed as a straight line as points are really close together
        curvature = (ds > 1e-6) ? (dtheta / ds) : 0.0;

        midline[i].kappa = curvature;
    }

}


int Map::get_midline_size(){
    return m_size;
}

int Map::get_raceline_size(){
    return r_size;
}

Point Map::get_raceline(int idx){

    if (idx < 0 || idx >= r_size) {
        Point empty = {0,0,0,0};
        std::cerr << "Error: Index " << idx << " is out of bounds.\n";
        return empty;
    }
    return raceline.at(idx);

}

Midpoint Map::get_midpoint(int idx){

    if (idx < 0 || idx >= m_size) {
        std::cerr << "Error: Index " << idx << " is out of bounds.\n";
        return {};
    }
    return midline.at(idx);

}

Point Map::get_associated_raceline(std::vector<Point> vertices, int& idx){
    /*
    Finds closest raceline point to set of vertices to allow path generation to the raceline.
    returns closest raceline point 
    */


    double min_dist = 1e9;
    double min_idx = -1;

    for(int i = 0; i < r_size; i++){
        
        for(const auto& vertex : vertices){

            double dist = euc_dist(vertex.x, raceline.at(i).x, vertex.y, raceline.at(i).y);
        
            if(dist < min_dist){
                min_dist = dist;
                min_idx = i;
            }

        }

    }
    idx = min_idx;
    return raceline.at(min_idx);
}

Midpoint Map::get_closest_midline(Point pose, int offset){

    double min_dist = 1e9;
    double min_idx = -1;

    for(int i = 0; i < m_size; i++){
    
        double dist = euc_dist(pose.x,midline.at(i).x,pose.y,midline.at(i).y);

        if(dist < min_dist){
            min_dist = dist;
            min_idx = i;
        }
    }

    if(min_idx+offset > m_size){
        int idx = min_idx + offset - m_size;
        return midline.at(idx);
    }

    return midline.at(min_idx+offset);
}
```

### **`planning_main.cpp`**
```
#include "planning_main.hpp"

#define _USE_MATH_DEFINES

using namespace std::chrono_literals;

class Planning : public rclcpp::Node
{
public:
  Planning() : Node("minimal_publisher"), CAR_WIDTH(0.25), DIST_PER_STEP(2.0), RES_PER_STEP(25),  VERT_PER_STEP(5), SAMPLE_SIZE(0)
  {
    marker_publisher_  = this->create_publisher<visualization_msgs::msg::Marker>("/vertices_markers", 10);
    drive_publisher_   = this->create_publisher<ackermann_msgs::msg::AckermannDriveStamped>("/drive", 10);
    traj_publisher_    = this->create_publisher<sensor_msgs::msg::PointCloud>("/traj_msg", 10);

    car_subscriber_    = this->create_subscription<nav_msgs::msg::Odometry>("/ego_racecar/odom", 5, std::bind(&Planning::car_callback, this, std::placeholders::_1));
    marker_sub_        = this->create_subscription<visualization_msgs::msg::Marker>("/vertices_markers", 10, std::bind(&Planning::marker_callback, this, std::placeholders::_1));

    publisher_timer_   = this->create_wall_timer(1000ms, std::bind(&Planning::publisher_timer_callback, this));

    if(!levine.file_found){
      RCLCPP_INFO(this->get_logger(), "WAS NOT ABLE TO GENERATE MIDLINE");
    }

  }

private:
  // Constants
  const double CAR_WIDTH, DIST_PER_STEP;
  const int RES_PER_STEP, VERT_PER_STEP;
  int SAMPLE_SIZE;
  double MAX_KAPPA;

  // Car data
  Car ego_car;

  // Map data
  std::string package_path = ament_index_cpp::get_package_share_directory("planning");
  std::string centreline_path = package_path + "/assets/centerline.csv";
  std::string raceline_path = package_path + "/assets/raceline.csv";
  Map levine = Map(centreline_path, raceline_path, MAX_KAPPA);

  // Lattice Data
  Lattice lattice = Lattice(levine, DIST_PER_STEP, VERT_PER_STEP, RES_PER_STEP, CAR_WIDTH, SAMPLE_SIZE);

  // Timers
  rclcpp::TimerBase::SharedPtr publisher_timer_;

  // Publishers
  rclcpp::Publisher<visualization_msgs::msg::Marker>::SharedPtr marker_publisher_;
  rclcpp::Publisher<ackermann_msgs::msg::AckermannDriveStamped>::SharedPtr drive_publisher_;
  rclcpp::Publisher<sensor_msgs::msg::PointCloud>::SharedPtr traj_publisher_;

  // Subscriptions
  rclcpp::Subscription<nav_msgs::msg::Odometry>::SharedPtr car_subscriber_;
  rclcpp::Subscription<visualization_msgs::msg::Marker>::SharedPtr marker_sub_;
  
  // Subscriber Callback
  void car_callback(const nav_msgs::msg::Odometry::SharedPtr msg);
  void publish_markers(); 
  void publisher_timer_callback();

  //trajectory calc
  bool on_raceline = false;
  int current_lane_idx = -1;

  std::vector<Point> local_traj;

  std::uint8_t getCost(const std::vector<Point>& traj, bool on_raceline);

  void generate_traj();
  void publish_traj();

  size_t point_count;
  void marker_callback(const visualization_msgs::msg::Marker::SharedPtr msg) {
    point_count = msg->points.size();
  }

};

void Planning::car_callback(const nav_msgs::msg::Odometry::SharedPtr msg)
{
  ego_car.update_values(msg->pose.pose);
}

void Planning::publisher_timer_callback(){

  // if(point_count == 0){
  //   publish_markers(); // Publish markers after generating points
  // }
  generate_traj();
  publish_markers();
}

std::uint8_t Planning::getCost(const std::vector<Point>& traj, bool on_raceline){
/*
  Designed around ros2 costmap_2d. Value from costmap is resized and then addtional heuristics
  are added. Value is clamped between 0-255. heuristics account for 

  returns value between 0-255: 

  0 - 152 -> no colision
  153 - 250 -> possibly a colision
  251 - 255 -> definitely a colision
  These values are adjusted for the below weights. See inflation chart for ros2 for occupancy weights.
  Calculate value by first adjusting normal values by weight. Then add max heuristic value to each value. 
*/

  // must add up to 1.0
  double w_o = 0.8; //occupancy weight
  double w_k = 0.05; //curvature weight
  double w_r = 0.15; //raceline weight

  std::uint8_t cost = 0;

  // set cost to be value form ros2 costmap when implemented
  double occupancy_value = 0;
  double raceline_value = on_raceline ? 0 : 255; // 0 if on raceline otherwise 255

  double kappa_value = 0;
  for(auto& point : traj) {
    //normalise kappa to add up to a max 225. 
    //Compares point kappa with max kappa from raceline and normalises to between between 0-255. 
    //Then gets divded by number of points so total sum is between 0-255.
    kappa_value += (( std::clamp(fabs(point.kappa), 0.0, MAX_KAPPA) / MAX_KAPPA) * 255) / traj.size(); 
  }

  cost = static_cast<std::uint8_t>( w_o*occupancy_value + w_k*kappa_value + w_r*raceline_value );

  return cost;
}

void Planning::generate_traj(){

  std::vector<std::vector<Point>> trajs;
  Point pose = Point(ego_car.get_x(), ego_car.get_y(), ego_car.get_theta(), 0.0, 0.0, 1.0); //define initial accel here

  //startup init
  if(current_lane_idx == -1){
    int idx = lattice.find_closest_vertices_idx(ego_car);
    lattice.generateCurve(pose, lattice.get_raceline_vertex(idx), RES_PER_STEP, local_traj);
    current_lane_idx = VERT_PER_STEP;
  }

  //check if car is on raceline idx
  trajs = current_lane_idx == VERT_PER_STEP ? trajs = lattice.getTrajectories(ego_car) : lattice.getTrajectories(ego_car, false, current_lane_idx);
  
  if (trajs.size() > 0) {


    std::uint8_t min_cost = 255; //max cost value
    std::vector<Point> min_traj;

    for(size_t i = 0; i < trajs.size(); i++){
      std::vector<Point> traj = trajs.at(i);
      std::uint8_t cost = i == VERT_PER_STEP ? getCost(traj, true) : getCost(traj, false);
      if(cost < min_cost){
        min_traj = traj; 
      }
    }

    for (auto& point : min_traj) {
      local_traj.push_back(point);
    }
        
    publish_traj();
  }

  if(local_traj.size() > RES_PER_STEP*4){
    local_traj.erase(local_traj.begin(), local_traj.begin()+RES_PER_STEP);
  }

  current_lane_idx = VERT_PER_STEP;

}

void Planning::publish_traj(){
  sensor_msgs::msg::PointCloud trajectories;

  trajectories.header.frame_id = "map";
  trajectories.header.stamp = this->get_clock()->now();

  // Resize for all points
  trajectories.points.resize(local_traj.size());

  // Create additional data channels
  sensor_msgs::msg::ChannelFloat32 theta_channel, kappa_channel, vel_channel, accel_channel, time_channel;
  theta_channel.name = "theta";
  kappa_channel.name = "kappa";
  vel_channel.name = "velocity";
  accel_channel.name = "acceleration";
  time_channel.name = "time";

  for(std::size_t i; i < local_traj.size(); i++){
    
    trajectories.points[i].x = static_cast<float>(local_traj[i].x);
    trajectories.points[i].y = static_cast<float>(local_traj[i].y);
    trajectories.points[i].z = 0.0f;

    theta_channel.values.push_back(static_cast<float>(local_traj[i].theta));
    kappa_channel.values.push_back(static_cast<float>(local_traj[i].kappa));
    vel_channel.values.push_back(static_cast<float>(local_traj[i].vel));
    accel_channel.values.push_back(static_cast<float>(local_traj[i].accel));
    time_channel.values.push_back(static_cast<float>(local_traj[i].time));
  }

  trajectories.channels.push_back(theta_channel);
  trajectories.channels.push_back(kappa_channel);
  trajectories.channels.push_back(vel_channel);
  trajectories.channels.push_back(accel_channel);
  trajectories.channels.push_back(time_channel);

  traj_publisher_->publish(trajectories);

}

//visulization
void Planning::publish_markers(){
  // visualization_msgs::msg::Marker marker;
  // marker.header.frame_id = "map";
  // marker.header.stamp = this->get_clock()->now();
  // marker.ns = "centre_line";
  // marker.id = 0;
  // marker.type = visualization_msgs::msg::Marker::POINTS;
  // marker.action = visualization_msgs::msg::Marker::ADD;
  // marker.scale.x = 0.2;  // Point size
  // marker.scale.y = 0.2;
  // marker.color.a = 1.0;
  // marker.color.r = 1.0;  // Red color
  // marker.color.g = 0.0;
  // marker.color.b = 0.0;
  // marker.points.clear();

  // for(int i=0; i<levine.get_midline_size(); i++){

  //   std::vector<double> midpoint = levine.get_midpoint(i);

  //   geometry_msgs::msg::Point point;
  //   point.x = midpoint.at(0);
  //   point.y = midpoint.at(1);
  //   point.z = 0.0;
  //   marker.points.push_back(point);
  // }
  // marker_publisher_->publish(marker);

  visualization_msgs::msg::Marker marker1;
  marker1.header.frame_id = "map";
  marker1.header.stamp = this->get_clock()->now();
  marker1.ns = "vertices";
  marker1.id = 1;
  marker1.type = visualization_msgs::msg::Marker::POINTS;
  marker1.action = visualization_msgs::msg::Marker::ADD;
  marker1.scale.x = 0.3;  // Point size
  marker1.scale.y = 0.2;
  marker1.color.a = 1.0;
  marker1.color.r = 1.0;  // purple
  marker1.color.g = 0.0;
  marker1.color.b = 1.0;
  marker1.points.clear();

  // for (int i = 0; i < SAMPLE_SIZE; i++) {
  //   std::vector<Point> vertices = lattice.get_vertice_set(i);
  //   for(const auto& vertex : vertices){
  //     geometry_msgs::msg::Point vertex_point;
  //     vertex_point.x = vertex.x;
  //     vertex_point.y = vertex.y;
  //     vertex_point.z = 0;

  //     // RCLCPP_INFO(this->get_logger(), "x=%.2f, y=%.2f, z=%.2f", vertex[0], vertex[1], 0.0);

  //     marker1.points.push_back(vertex_point);
  //   }

  //   Point raceline_point = lattice.get_raceline_vertex(i);
  //   geometry_msgs::msg::Point vertex_point;
  //   vertex_point.x = raceline_point.x;
  //   vertex_point.y = raceline_point.y;
  //   vertex_point.z = 0;

  //   marker1.points.push_back(vertex_point);
  //  }
  
  // marker_publisher_->publish(marker1);

  // visualization_msgs::msg::Marker marker2;
  // marker2.header.frame_id = "map";
  // marker2.header.stamp = this->get_clock()->now();
  // marker2.ns = "raceline";
  // marker2.id = 2;
  // marker2.type = visualization_msgs::msg::Marker::POINTS;
  // marker2.action = visualization_msgs::msg::Marker::ADD;
  // marker2.scale.x = 0.2;  
  // marker2.scale.y = 0.2;
  // marker2.color.a = 1.0;
  // marker2.color.r = 0.0;  
  // marker2.color.g = 0.6;
  // marker2.color.b = 0.53;
  // marker2.points.clear();

  // for(int i=0; i<levine.get_raceline_size(); i++){

  //   Point raceline = levine.get_raceline(i);
  //   RCLCPP_INFO(this->get_logger(), "kappa: %f", raceline.kappa);

  //   geometry_msgs::msg::Point point;
  //   point.x = raceline.x;
  //   point.y = raceline..y;
  //   point.z = 0.0;
  //   marker2.points.push_back(point);

  // }
  // marker_publisher_->publish(marker2);

  visualization_msgs::msg::Marker marker3;
  marker3.header.frame_id = "map";
  marker3.header.stamp = this->get_clock()->now();
  marker3.ns = "raceline";
  marker3.id = 2;
  marker3.type = visualization_msgs::msg::Marker::POINTS;
  marker3.action = visualization_msgs::msg::Marker::ADD;
  marker3.scale.x = 0.2;  
  marker3.scale.y = 0.2;
  marker3.color.a = 1.0;
  marker3.color.r = 0.0;  
  marker3.color.g = 0.6;
  marker3.color.b = 0.53;
  marker3.points.clear();

  for(const auto& point : local_traj){

    geometry_msgs::msg::Point curve_point;
    curve_point.x = point.x;
    curve_point.y = point.y;
    curve_point.z = 0;

    marker3.points.push_back(curve_point);

  }
  marker_publisher_->publish(marker3);

}

int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<Planning>());
  rclcpp::shutdown();
  return 0;
}
```

```
#ifndef COMMON_H_
#define COMMON_H_

#define _USE_MATH_DEFINES
#include <cmath>


struct Point {
    double x, y, theta, kappa, vel, accel, time;

    // Default constructor (initializes everything to 0)
    Point() : x(0), y(0), theta(0), kappa(0), vel(0), accel(0), time(0) {}

    // Parameterized constructor with default values for optional parameters
    Point(double x, double y, double theta, double kappa, double vel = -1, double accel = -1, double time = -1)
        : x(x), y(y), theta(theta), kappa(kappa), vel(vel), accel(accel), time(time) {}
};

struct Midpoint{
    double x, y, theta, kappa, w_i, w_o;
};

inline double euc_dist(double x1,double x2,double y1,double y2){
    return std::sqrt(std::pow(y2 - y1, 2) + std::pow(x2 - x1, 2));
}

inline double normalise_angle(double theta){
    double normalised_theta = theta;
    if(theta > M_PI){
        normalised_theta = theta - 2.0 * M_PI;
    }
    else if(theta < -M_PI){
        normalised_theta = theta + 2.0 * M_PI;
    }

    return normalised_theta;
}

#endif
```

# Summary

Here’s a high-level structural overview and important details of the F1Tenth Lattice Planner implementation, based on the provided files:

---

## **High-Level Structure**

### **Main Classes and Data Types**

| Class / Struct | File | Purpose / Key Data Members |
|---|---|---|
| **Car** | car.cpp | Represents the ego vehicle’s pose and orientation. Maintains position $(x, y, z)$, heading $\theta$, orientation quaternion $q$, and unit vectors for forward, lateral, and vertical axes. |
| **Map** | map.cpp | Loads and stores track information from CSV files. Holds midline and raceline as vectors of `Midpoint` and `Point` structs. Computes angles and curvatures for the track. |
| **Lattice** | curve_gen.cpp | Generates the lattice structure (vertices), computes candidate trajectories (curves) between vertices, and provides trajectory generation and refinement (Newton’s method for cubic spirals). |
| **Planning (ROS Node)** | planning_main.cpp | Main ROS2 node. Handles subscriptions (car pose), publishes markers and trajectories, manages the planning loop, and selects the best trajectory based on a cost function. |

#### **Supporting Data Types**
- **Point**: Likely a struct with $x, y, \theta, \kappa, v, a, t$ (position, heading, curvature, velocity, acceleration, time).
- **Midpoint**: Struct with $x, y, \theta, \kappa, w_i, w_o$ (position, heading, curvature, inner/outer widths).

---

## **Algorithm Flow**

1. **Map Loading** (`Map`)
   - Loads midline and raceline from CSVs.
   - Computes heading ($\theta$) and curvature ($\kappa$) for each point.

2. **Lattice Generation** (`Lattice`)
   - Samples the midline at intervals (`DIST_PER_STEP`).
   - At each sample, generates a set of vertices laterally across the track width (`VERT_PER_STEP`), offset by inner/outer track widths.
   - Associates each vertex group with the closest raceline point.

3. **Trajectory Generation** (`Lattice`)
   - For a given car pose, finds the closest vertex group.
   - For each target vertex in the next group, generates a cubic spiral trajectory from the current pose to the target.
   - Uses Newton’s method to iteratively refine the spiral parameters so the endpoint matches the target (minimizing error in $x, y, \theta, \kappa$).

4. **Trajectory Selection** (`Planning`)
   - For each candidate trajectory, computes a cost:
     - **Occupancy** (placeholder, for future costmap integration)
     - **Curvature** (penalizes high curvature)
     - **Raceline** (penalizes deviation from raceline)
   - Selects the trajectory with the lowest cost.

5. **Publishing**
   - Publishes the selected trajectory as a `PointCloud`.
   - Publishes visualization markers for vertices and the selected trajectory.

---

## **Important Implementation Details**

- **Car Orientation Handling**: The `Car` class updates orientation using quaternions and computes the heading angle from the forward vector.
- **Lattice Structure**: Vertices are generated laterally across the track at each sampled midline point, allowing for multi-lane planning.
- **Curve Generation**: Uses cubic spirals (parametric curves) for smooth, feasible paths. Newton’s method is used to refine the spiral parameters so the endpoint matches the target vertex.
- **Cost Function**: Modular, with weights for occupancy, curvature, and raceline adherence. Designed to be compatible with ROS2 costmap_2d.
- **Trajectory Buffering**: Maintains a local trajectory buffer, erasing old points as the car progresses.
- **ROS2 Integration**: The `Planning` class is a ROS2 node, handling all subscriptions, publications, and timers.

---

## **Summary Table**

| Component | Key Responsibilities |
|---|---|
| **Car** | Track and update ego vehicle pose and orientation. |
| **Map** | Load and process midline/raceline, compute angles/curvatures. |
| **Lattice** | Generate lattice vertices, compute and refine candidate trajectories. |
| **Planning** | ROS2 node: subscribe to car pose, generate/select/publish trajectories, visualize. |

---

## **Notable Points**

- **Newton’s Method for Curve Refinement**: Ensures that the generated spiral trajectory ends exactly at the target vertex with the desired pose and curvature.
- **Track Representation**: Both midline and raceline are used; raceline is used for optimal path reference, midline for lattice sampling.
- **Extensibility**: The cost function is designed to be easily extended with additional heuristics (e.g., dynamic obstacles, speed limits).
- **Visualization**: Multiple marker types are prepared for debugging and visualization in RViz.

---

If you need a diagram or more details on a specific class or function, let me know!

# Spiral Gen
The `generateSpiral` function in `curve_gen.cpp` generates a discrete cubic spiral path from a given start point, using cubic polynomial curvature coefficients, and returns the final state of the spiral. Here's a detailed explanation of how it works:

---

### Function Signature

```cpp
Point Lattice::generateSpiral(Point start, double a, double b, double c, double d, double sf, int steps, std::vector<Point>& points)
```

- **Inputs:**
  - `start`: The starting `Point` of the spiral (with position, heading, velocity, acceleration, etc.).
  - `a, b, c, d`: Cubic polynomial coefficients defining curvature $\kappa(s) = a + b s + c s^2 + d s^3$ along the path length $s$.
  - `sf`: The total arc length of the spiral.
  - `steps`: Number of discrete steps to divide the spiral into.
  - `points`: A vector to which the generated discrete points along the spiral will be appended.

- **Output:**
  - Returns a `Point` representing the final state at the end of the spiral (position, heading, curvature).

---

### How It Works

1. **Step Size Calculation:**

   ```cpp
   double ds = sf / steps;
   ```

   The total spiral length `sf` is divided into `steps` equal increments of length `ds`.

2. **Initialization:**

   ```cpp
   double x = start.x, y = start.y, theta = start.theta, accel = start.accel;
   double vel, time;
   ```

   Initialize the current position $(x,y)$ and heading $\theta$ to the start point's values. Also retrieve acceleration.

3. **Iterative Spiral Generation:**

   The function iterates from `i = 1` to `steps`, incrementally computing the state at each step along the spiral:

   ```cpp
   for (int i = 1; i <= steps; ++i) {
       double s = i * ds;  // current arc length along spiral
       double kappa = a + b * s + c * s * s + d * s * s * s;  // curvature at s
       theta += kappa * ds;  // update heading by integrating curvature over ds
       x += cos(theta) * ds; // update x by moving ds along heading
       y += sin(theta) * ds; // update y similarly
       
       // velocity update assuming constant acceleration
       vel = (start.vel + 2*accel*s) >= 0 ? std::sqrt(start.vel + 2*accel*s) : -1;
       
       // time update depending on acceleration
       time = accel == 0 ? sf/vel : (vel - start.vel)/accel;

       // create a Point for this step and add to points vector
       Point point = {x, y, theta, kappa, vel, accel, time};
       points.push_back(point);
   }
   ```

   - **Curvature $\kappa(s)$** is evaluated from the cubic polynomial at the current arc length `s`.
   - **Heading $\theta$** is updated by integrating curvature over the small step `ds` (since curvature is the derivative of heading w.r.t. arc length).
   - **Position $(x,y)$** is updated by moving forward `ds` along the new heading.
   - **Velocity and time** are updated assuming constant acceleration.
   - Each computed state is stored as a `Point` in the output vector.

4. **Return Final State:**

   After the loop, the function returns the final state as a `Point`:

   ```cpp
   return {x, y, theta, d};
   ```

   Note that the curvature returned here is `d`, the cubic coefficient of the highest order term, which may be used downstream.

---

### Summary

- `generateSpiral` numerically integrates a cubic curvature profile to produce a smooth path.
- It discretizes the path into small steps, updating position and heading by integrating curvature.
- It also tracks velocity and time assuming constant acceleration.
- The generated points represent a smooth cubic spiral trajectory from the start point over length `sf`.

This function is a core part of the lattice planner's trajectory generation, enabling smooth, feasible paths between lattice vertices.

---

If you want, I can also explain how this fits into the iterative refinement in `generateCurve`.

# Important Interesting Points
- generateCurve returns by reference the list of points, only single point in the function call
- Generate curve calls generate spiral w/ current guess of spiral params
	- For each final state found, gen curve checks the cost to iteratively change the spiral param to lower cost below some tolerance

Vertice groups: laterally sampled verticies