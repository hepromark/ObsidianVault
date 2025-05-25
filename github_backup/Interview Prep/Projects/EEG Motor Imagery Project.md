#### What is it?
- A deep learning model trained on an EEG motor imagery dataset

#### What's the data?
- 109 subjects, where the subjects are asked to to move their hands/feet and then *think* about moving their hands and feet
	- 4 classes: 
		1. Open & close left OR right fist
		2. Imagine opening & closing left OR right fist
		3. Open and close both fists or both feet
		4. Imagine opening and closing both fists or both feet
		5. Baseline nothing
- 64-channel EEG readings: 64 x (160 samples/sec * 2 minutes) = 2D array per subject / trial run

#### What's the model?
- We looked at using sequential-based models (RNNs,, LSTMs) and transformers, but we settled on CNNs
- Weird to use CNN for time series data?
	- Yes, but turn into spectrographs first and stack channel-wise
	- Ends up with 3D volume per input sample

#### What's the result?
- ~60% percent accuracy, not too good
- Reasons: We didn't do LPF on the input data, we haven't tuned our Fast Fourier Transform window size too much
- I plan on continuing the project after my signals course

What I learned:
- Fast Fourier Transform
- Some spectrography ideas
- h5py file type
	- I trained it locally, so I need a memory-efficient way to store 3D arrays
	- Different batching methods so that the train step is more efficient
	- We want *some* randomness so that train doesn't get overfitted to the current group of training examples, but *not too much* such that memory loading takes too long.