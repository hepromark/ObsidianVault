Code: https://github.com/UWARG/computer-vision-python/blob/main/modules/cluster_estimation/cluster_estimation.py

### Project Overview

**Description:** Cluster estimation model for a drone that filters out outlier & false positive landing pad predictions.
**Goal:** The autonomy team needed a way to remove costly false positives landing pads from the detection set, using temporal state information (i.e. outliers don't get detected consistently over time)
### Timeline & Scope

**Timeline**: Over 1 term (4 months) , I collaborated with 1 other member with guidance from a senior lead.
- It's still currently used in the repository & updated by newer members
### Technical Stack

**Languages**: Python
**Libraries**: NumPy, Matplotlib for visualisation, Scikit-learn for ML models

### System Design

The clustering module is placed right after the landing pad detector module & geotag module, and outputs to the mission planner to use

Pad detection -> Geotagging Module -> *Clustering module* -> Mission Planner

### Key Features & Functionality

The model uses a [[Variational Gaussian Mixture Model]] (https://scikit-learn.org/0.15/modules/dp-derivation.html) to do clustering of data points. 

The module's goal is simple: given a list of points, output the centroids (center of gravity in xy space) of these clusters
- We need to filter out outlier detections that are far away from other points
- And keep a running list of likely centroids

### Technical Challenges

1. Defining assumptions to what 'good enough' meant in this context:
	- This is essentially a filter, but we want it to work in low # detection cases well -> lots of hyper-param tuning to make the filter robust in low data point cases
	- Defining covariance maxes that are physically informed (ex: how accurate is the incoming data, at what point is a detection an outlier and at what point is that detection part of a centroid)
	- Choosing when a certain centroid is "outlier" -> there is some weight threshold where before that we assume it's an outlier, but the datapoints must be retained and corrected in-case it turns out to be a valid pad that we are gradually observing
### Results

Used in WARG's 2023 competition, was used onboard for 2025's Competition win.

### Learning

1. How cluster estimation, K-means clustering, and VGMM clustering works
2. Test-driven Software Development
3. SWE development within a team, PRs, etc.

### Future Improvements

Further fine-tuning on real world datasets (competition datasets)

Use models that somehow incorporate temporal data directly within the clustering process:
- Right now it's hand-defined filtering, using domain expertise and fine tuning
- Perhaps some way to time-stamp and cluster the points in 'spatial-temporal' dimensions could work better
- Outliers are likely also outliers in the temporal axis 
