What is Supervised Learning?

It is a subset of [[Machine Learning Paradigms |Machine Learning]] where the input training data is labelled. The machine learning model takes input and generates an output corresponding to this input. 

**The objective** is to make accurate predictions (outputs) on unseen data, i.e. *generalize* from training data -> real world data.

#### Regression Problems
- Model outputs continuous values
- Ex: housing price prediction -> output is a price
- One of the most common is the [[Linear Regression]] algorithm, which fits a line to the data
- [[Logicistic Regresion]] is more complicated, which fits a sigmoid function to the data.

#### Classification Problem
- Model outputs a discrete value from a finite set of possible values
- Ex: Animal classifier -> output is an animal name, from a list of possible names

Both regression and classification are architecturally *very similar*:
- Last layer output for regression has no / linear activation or a [[Activation Function |ReLU]] unit
- Classification same architecture throughout, except last layer has $N$ nodes for the $N$ classes, with a [[Activation Function|Softmax]] output


