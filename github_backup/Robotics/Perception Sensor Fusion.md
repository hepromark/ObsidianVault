Sensor fusion is used in perception systems w/ more than 1 type of perception sensor:
- Lidar + RGB for cars
- RGB + depth for stereo vision

### Classical Methods

1. Projection
	- Ex: Lidar + RGB
	- Project LiDAR points into RGB camera image plane
	- Makes object detection / segmentation better

2. Rule-based fusion
	- A mix of heuristics + filters
	- Ex: Valid = object is detected in RGB + LiDAR sees 3D cluster
	- Combine LiDAR clusters w/ bounding boxes from camera (IoU matching, center point matching)
	- Depth priors to improve detection ranges
	- False positive filtering (ex: remove lidar reflection by checking RGB)

3. Probabilistic Models
	- [[Kalman Filters]]
	- Joint Probabilistic Data Association JPDA

### Learning-based Methods

Fusion can happen at 3 stages:
- Early Fusion - raw sensor inputs merged *before* feature extraction
	- Ex: Concatenate depth + rgb channels -> encoder
	- PointPainting
- Mid Fusion - Extract features from each channel, then fuse
	- Ex: [[Convolutional Neural Networks|CNN]] encoder for image, [[PointNet]] for LiDAR
	- Fuse feature maps
	- BEVFusion, MV3D
- Late Fusion - Make independent predictions, then merge results
	- Merge bounding boxes from RBG & Lidar detectors
	- Frustum PointNets