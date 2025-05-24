Specifically chose the **Variational Bayesian Gaussian Mixture** - wanted a simple way to cluster landing pad detections

- weight_concentration_prior low and the specified component count is higher than actual centroids, model can “ignore” any unused components
- regularization by adding information from prior distributions
- Extra hyperparameters to tune
- slower inference → Object detection is bottleneck, low # of data points so speed not an issue