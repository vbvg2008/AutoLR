Objective:
===
Automatic learning rate scheduling


How does it work
===
training history + validation history (optional)  -> LRcontrolNet -> next lr (or next action to change lr)


Hypothetical outcome
===
* faster convergence (to reach same goal)
* better results (given same computational resource)

Potential Impact
===
* single model training becomes easier & faster
* AutoML can discover more potential in each network architecure




Details:
===

### what does the network predict? (several options)
* classification, predict action class (each means a different action, say 1.1lr, 1.0lr, 0.9lr)
* regression, (would be hard to collect the data, and there's nothing as "ideal lr")


### what's the input format?
* 1D time series (training), 1D time series(validation, optional)


### the loss magnitude varries across different training, how to stndardize them?
* divide by initial loss
* divide by previous loss value


### how often should we run the autoLR controller?
* can't run too frequently because the inferencing will take some time
* can't wait too long because LRController needs to control the training

### what network architecture to work on 1D?
* 1D convolution? maybe
* tabular dense connection
* RNN based model

### Network requirements:
* network has to be small enough, because inferencing time and gpu memory is a constraint



Limitations:
===
* this AutoLR control should only work when the loss function is not changing throughout the training (in other words, can't use it on GAN stuff)


Data Gathering:
===
Experiment design (gathering data):
1. randomly choosing an initial learnign rate on log scale (between 1e-1 to 1e-4)
2. run the training for 100 steps, record losses for every step
3. save model
4. load model save in 3, perform following 3 decisions:
	a. lr*0.9 for 100 steps, evaluate accuracy 98%
	b. lr*1.0 for 100 steps, evaluate accuracy 98%
	c. lr*1.1 for 100 steps, evaluate accuracy 98.1%
5. evaluate the accuracy, generate ground truth
6. load model save in 3, randomly select an action, repeat 2-5 until training ends



Other notes:
===
* short term goal vs long term goal
* simiar to DQN, estimate what's the loss from the AutoLR controller