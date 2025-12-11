# Background

**Foundational Model Definition:** Models pre-trained on huge datasets and can be fine-tuned for downstream tasks
- Other fields (Vision, NLPs) have significant breakthrough models
- Billions+ parameters

Traditional DL models require specific data to the task, which are not as numerous
- Either 1) get lots of data on every task you want or
- 2) bad at the task

**Potential Benefits**
1. Better generalisation ability
2. Zero-shot solutinos to problems not in training data
3. Inherent multi-modality from its training data being multi-modal

**Main Challenges**
1. Data scarcity: getting internet-scale data for manipulation, locomotion, nav, etc. is hard because robot-specific data is scarce
2. High variability: different physical environments, tasks, robot platforms
3. Hallucinations (from LLMs) won't work on robots due to physical limitations
4. Real-time performance: high inference time for foundation models hard to use in real-time robotic apps

### Terminology

**Tokenization**:
- Byte-pair encoding of characters/words/phrases (depends on model & implementation)

**Generative Models:**
- Model that learns & samples from prob distribution to create examples of data from same distribution
- Can be trained to be *conditional*, meaning they generate from a *conditioned distribution* conditioned on some info (ex: a face gen model conditioned on female faces -> generates female faces)

**Discrimitive Models:**
- Regression or classification tasks
- Trained to find difference between classes or categories
- Tries to learn *boundaries* between classes inside input space

**Autoregressive Models:**
- A representation of random processes whose outputs depend casually on previous outputs
- Uses a window of past data to predict next data point in a sequence
- LSTMs, RNNs are learnable non-linear autoregressive models

**Masked Auto-Encoding**:
- Masks a % of tokens in the corpus & requires model to predict these tokens
- Model is encouraged to learn context that surrounds a word, rather than just the next word in a sequence

**Contrastive Learning**
- VLMs rely on different training methods than LLMs called contrastive representation learning
- Goal: learn a *joint embedding space* between input modalities where similar sample pairs are closer than different ones
- Encourages text & image encoders to preserve mutual info between true text and image pairs

**Diffusion Models**
- Outside of LLMs and VLMs, diffusion models are used for image gen
- Essentially the models learn 2 markov chains (forward and reverse) that 1) adds Gaussian noise to input and 2) learns to remove noise

# Robotic Applications

## Robotic Policy Learning

Focuses on letting robots make decisions and execute actions in their environment
- Covers perception -> decision making -> control
- Foundation models very useful in this field
- Involves training a robot to choose appropriate actions **(policies)** given its current state and a goal

2 main types:
### 1. Language-conditioned Imitation Learning

A goal conditioned policy is learned, to output actions based on the *state and language instruction*

In the dataset:
- Demonstrations (training examples) can be represented as trajectories, sequences of images, RGB-D voxel observations, etc.
- Language instructions are then paired w/ demonstrations to be used in the training dataset

At test time:
- Robot given a series of instructions
- Policy gives actions in a closed-given actions at each time step

Main challenges:
1. Obtain a sufficient volume of demonstrations and conditioning labels to train a policy
2. distribution shift under the closed-loop policy (feedback nature can lead robot into regions of the state space that isn't covered well by the data)

How Foundational Models are used:
1. Learning from Play Data: **Play-MLP** proposes learning resuable latent plan represetations from unstructured, unlabeled, and cheap-to-collect teleoperated play data

2. Multi-Context Imitation **(MCIL)**:
	- Uses language-conditioned imitation learning over unstructured data
	- Trains a single latent goal-conditioned policy across multiple different datasets (ex: image goals, language goals, one-hot tasks) simulatenously
	- Encodes contexts in the shared latent space using associated encoder for each context

3. Foundation Model Feedback/Labelling:
	- Pretrained foundation models can provide feedback by labelling demonstrations, generate paired instruction-demo data for policy fine-tuning

### 2. Language-assisted Reinforcement Learning

# How are they used

Trending towards producing intermediate representations, or a 'layered approach' where thinner models are built ontop of the foundational model depending on the application

### Common Representations:

**1. Intermediate Representations / Latent Spaces:**
- Many models learn compact, homogenous representations by fusing & aligning multimodal data (vision, language, etc.)
- The output representation captures semantic, spatial, temporal and "properties of an object" info
- Ex: *CLIP* learns a join embedding space between images and text, where similar pairs are closer

**2. High-Level planning & reasoning**:
- LLMs generates code for task planning, natural language instructions -> temporal logic, or provide feedback/reward signals for policy learning

**3. Direction Action or Low-Level Control Outputs**:
- VLA models like Robot Transformer (RT-1, RT-2) can output discretized base and arm actions
- PerAct output discretize actions by detecting next voxel action
- [[GR00T N1]] (Nvidia humanoid foundation model) is a VLA model designed to output fluid motor actions in real time 

### Training on top of Foundational Models

Common practice to use foundation models as foundation (freeze weights), and train a specialized model on top to handle specific low-level control / task execution
- Ex: GR00T N1 uses pre-trained VLM (frozen during policy training) to give vision-language tokens
- Tokens fed into Diffusion Transformer module, w/ embodiment-specific encoders and decoders, to generate motor actions
- Also does post-training fine-tuning on embodiment-specific data to adapt to new tasks efficiently