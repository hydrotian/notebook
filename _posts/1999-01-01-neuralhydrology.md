---
layout: post
title: Learning Neural Hydrology
author: Tian
comments: true
categories: []
tags: Null
developing: true
---
I want to take some notes when learning [Neural Hydrology](https://neuralhydrology.github.io/) for some research ideas. Since some of the information in this note is not verified or based on some conversations with ChatGPT, I don't want list this as an open note. But I may sometime when I have 100% confidence about the content.

## Neural Hydrology
The tutorial of [Neural Hydrology](https://neuralhydrology.readthedocs.io/en/latest/index.html) is super helpful! Some quick steps and tips:
- Step 1: pretty standard, install miniconda and the environments for either cpu or gpu.
    - If on windows desktop with CUDA gpu, check task manager -> GPU -> Cuda for performance chart
    - on NERSC, I need to `load conda` first
    - After creating environments, use `conda activate nh-gpu` to activate the environment.
    - on NERSC, if using [Jupyter notebook](https://jupyter.nersc.gov/), make sure to create a new Python Kernel first and then change the default kernel to the new one created following [this page](https://docs.nersc.gov/services/jupyter/how-to-guides/). 
- Step 2: download the [data for the tutorial](https://neuralhydrology.readthedocs.io/en/latest/tutorials/data-prerequisites.html). These are CAMELS datasets, pretty useful.

## Some explainations for config options (not verified)
While some of the config options are self-explanatory, some options relavent to LSTM needs more explainations. I will just add them as comments.

<pre class="wrap-code"><code>
# which GPU (id) to use [in format of cuda:0, cuda:1 etc, or cpu or None]
## TZ: on desktop, if only one video card, then it's cuda:0
device: cuda:0 

# --- Validation configuration ---------------------------------------------------------------------

# specify after how many epochs to perform validation
validate_every: 3

# specify how many random basins to use for validation
validate_n_random_basins: 1

# specify which metrics to calculate during validation (see neuralhydrology.evaluation.metrics)
# this can either be a list or a dictionary. If a dictionary is used, the inner keys must match the name of the
# target_variable specified below. Using dicts allows for different metrics per target variable.
metrics:
  - NSE

# --- Model configuration --------------------------------------------------------------------------

# base model type [lstm, ealstm, cudalstm, embcudalstm, mtslstm]
# (has to match the if statement in modelzoo/__init__.py)
## TZ: Some of the available models include 'cudalstm' (CUDA-accelerated LSTM), 'ealstm' (Entity-Aware LSTM), 'mtslstm' (Multi-Timescale LSTM), 'transformer' (based on the Transformer architecture), 'odelstm' (ODE-LSTM), and 'mclstm' (Mass-Conserving LSTM). Each model has distinct characteristics and applications, so the choice depends on the specific requirements and objectives of your hydrological modeling task.
model: cudalstm

# prediction head [regression]. Define the head specific parameters below
## TZ: The head option in NeuralHydrology configures the prediction layer of the model (or final layer where the actual predication is made). Options include 'regression' for standard output, 'gmm' for Gaussian Mixture Models, 'cmal' and 'umal' for Censored and Uncensored Mixture Approximation of Location-scale distributions, respectively. 
head: regression

# ----> Regression settings <----
## TZ: The output_activation refers to the activation function used on the output layer of the model. It determines how the raw output from the network is transformed. Common types include 'linear', 'ReLU', and 'softplus', each shaping the output differently, affecting predictions and model performance. ReLU (Rectified Linear Unit) is an activation function that outputs the input directly if it is positive; otherwise, it outputs zero. It introduces non-linearity, helping the network learn complex patterns without affecting positive values, which is useful for various types of neural networks, including those predicting non-negative values like runoff.
output_activation: linear

# ----> General settings <----

# Number of cell states of the LSTM
## TZ: The hidden_size option specifies the number of units in the hidden layers of the model. It's a crucial parameter that impacts the model's capacity to learn and represent data. A larger hidden_size allows the model to capture more complex patterns but also increases computational complexity and the risk of overfitting. The optimal size often depends on the dataset's complexity and the specific model architecture you're using. Hidden layers consist of a complex arrangement of cells and gates that process sequential data. These layers contain matrices of weights and biases that are updated during training. While the exact representations within these layers are not directly interpretable like the final output, they encode patterns and relationships learned from the input data (like daily precipitation) to help the model make accurate predictions (like daily runoff).
hidden_size: 20

# Initial bias value of the forget gate
## TZ: Setting this bias can help in learning by determining how much of the previous state to retain. A positive initial forget bias encourages the model to remember more of the past state initially
initial_forget_bias: 3

# Dropout applied to the output of the LSTM
## TZ: Dropout is a regularization technique used to prevent overfitting by randomly setting a fraction of the output units to zero during training. This helps the model to learn more robust features that are not reliant on any small set of neurons. The 'output_dropout' setting controls the proportion of outputs that are dropped.
output_dropout: 0.4

# --- Training configuration -----------------------------------------------------------------------

# specify optimizer [Adam]
## TZ: The 'optimizer' in a neural network is an algorithm that updates the weights and learning parameters to minimize the loss function. It determines how the network learns from the data and improves its predictions over iterations. Common optimizers include SGD, Adam, and RMSprop, each with different strategies for adjusting weights and handling the learning rate. Adam (Adaptive Moment Estimation) is the only option for neural hydrology. It combines ideas from RMSprop and momentum. It calculates adaptive learning rates for each parameter by keeping track of the exponentially decaying average of past gradients (momentum) and squared gradients (scale). This helps it navigate the parameter space more effectively, often leading to faster and more stable convergence in training neural networks.

## TZ: About Adam, ChatGPT says: Adam is generally considered a local optimizer. It's designed to efficiently find a local minimum in high-dimensional spaces, primarily used in deep learning where the landscape is complex and has many parameters. It doesn't inherently have mechanisms to escape local minima and find the global minimum like some global optimization methods do. In complex landscapes, while Adam may not guarantee a global minimum, it's often effective due to its adaptive learning rate and momentum. It's trusted because it usually reaches good solutions quickly and can navigate difficult areas where other optimizers might get stuck. However, for critical applications, it's common to run multiple initializations or combine Adam with other strategies to better explore the parameter space and increase confidence in the results. In most LSTM applications, Adam is typically deployed just once per training session, iterating through the dataset multiple times in epochs. However, for more robust results, especially in complex scenarios, it's not uncommon to run the entire training process multiple times with different initializations.
optimizer: Adam

# specify loss [MSE, NSE, RMSE]
loss: MSE

# specify learning rates to use starting at specific epochs (0 is the initial learning rate)
## TZ: The 'learning_rate' is a hyperparameter in optimization algorithms like Adam that determines the step size at which the model's parameters are updated during training. It controls the magnitude of parameter updates in each iteration, affecting how quickly or slowly the model converges to an optimal solution. A higher learning rate can make training faster but risk overshooting the minimum, while a lower learning rate may converge more slowly but with greater stability. The appropriate learning rate depends on the specific task and data.
learning_rate:
  0: 1e-2
  30: 5e-3
  40: 1e-3

# Mini-batch size
## TZ: In deep learning, training is often done with mini-batches, which are subsets of the entire training dataset. A batch size of 256 means that, during each training iteration, the model processes 256 examples from the dataset together, calculates the gradient of the loss function, and updates its parameters. Using mini-batches helps to speed up training and stabilize the optimization process compared to using the entire dataset in each iteration. If your input dataset consists of multiple daily time series, using a batch size of 256 means that in each mini-batch, the training process will randomly select 256 individual time steps. These time steps could come from different time series but represent the same day across those series. The model would use these time steps and their corresponding values to make predictions. This approach allows the model to learn patterns and relationships across different time series while maintaining the daily structure within each mini-batch. 
batch_size: 256

# Number of training epochs
epochs: 50

# If a value, clips the gradients during training to that norm.
clip_gradient_norm: 1

# Defines which time steps are used to calculate the loss. Can't be larger than seq_length.
# If use_frequencies is used, this needs to be a dict mapping each frequency to a predict_last_n-value, else an int.
predict_last_n: 1

# Length of the input sequence
# If use_frequencies is used, this needs to be a dict mapping each frequency to a seq_length, else an int.
seq_length: 365

# Number of parallel workers used in the data pipeline
num_workers: 8

# Log the training loss every n steps
log_interval: 5

# If true, writes logging results into tensorboard file
log_tensorboard: True

# If a value and greater than 0, logs n random basins as figures during validation
log_n_figures: 1

# Save model weights every n epochs
save_weights_every: 1

</code></pre>
 