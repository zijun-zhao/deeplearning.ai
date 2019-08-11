## 27July

1. Input feature vector x is also the activations of layer 0

2. Andrew didn't think there exist any better way than explicit for-loop to implement each layer from 1 to L.

3. Figuring out the dimensions of the various matrices you will be working with is really helpful.

4. Main intuition taken away from the facial detection example is that finding simple things like edges and then building them up composing them together to detect more complex things.

5. Similarly, when building a speech recognition system, you need to know how to visualize speech but if only input an audio clip, the first level of the neural network might detect some low level audio waveform features: tone going up or down, white noise or sibilant sound lights, then by combining those low-level waveform then it can detect the basic units of sound(in linguistics it is called phonemes音位), then maybe able to learn work, then learn sentence.

6. Human brain may also start from learning simple things.

7. Another intuition of *why deep neural network works well* comes from circuit theory: (Informally) there are functions you can compute with a "small" L-layer deep neural network that shallower networks require exponentially more hidden units to compute. Here small refers to the number of hidden unit in each layer.

8. Andrew thought deep learning is just neural networks with a lot of hidden layers rebranded.

9. Given the derivative w.r.t. da<sup>[l]</sup>, how much do i wish a<sup>[l-a]</sup> may change will show in da<sup>[l-1]</sup>.

10. Concepturally it will be helpful to think of the cache as to store the value of z, but when implementing, you may find that cache is a useful way to get the value of the parameters at W<sup>[l]</sup> and b<sup>[l]</sup> into the backward propagation.

## 28July 

1. When you are training deep nets, being able to organize your hyperparameters will be helpful for you to be more efficient in developing the networks

2. Hyperparameters we have already learned are 
	* learning rate \alpha
	* \# iterations
	* hidden laters L
	* hidden units
	* choice of activation function
	
	Later we will learn other hyperparameters such as *momentum*, *mini batch size*, various *form of regularization prameters*.

3. To be precise, the learning rate alpha is a parameter that determines the real parameters.

4. To apply deep learning, the process is very empirical. First you have an idea(best value for learning rate), then try out and see how it works. If not sure, you can try different versions of learning rate and see the tendency of the cost function.

5. *Idea-> Code -> Experiment*: Applied deep learning is a very empirical process, you need to try a lot of things and see what works.

6. Area that have been explored by deep learning includes computer vision, speech recognition, natural language process, advertisement and web search or product recommendations. For people new to a certain region, Andrew suggested to try out a range of values and see what works.

7. Even for those one who are familiar for a given area, one rule of thumb is to **try a few values for the hyperparameters and double check if there is a better value for the haper parameters**. Because the CPU and GPU and datasets are changing all the time, just keep trying and evaluating them on a hold out cross-validation set or sth. else. As you do so, you will have more intuition about the choice for hyperparameters.

8. What does deep learning has to do with a brain? Actually Andrew's answer is "not a whole lot".

9. Andrew believed that today even neurocientists have almost no idea what even a single neuron is doing; How neurons in the human brain learn is still a mysterious process.

10. Deep learning is a good tool to learn very flexible and comples functions: x->y in supervised learning. The anology of brain is not encouragebale.

## Quiz

1. Even using vectorization, we still need to compute forward propagation in an L-layer neural network with an explicit for-loop (or any other explicit iterative loop) over the layers l=1, 2, …, L.

2. **Weight matrices and bias vectors are **parameters**.

3. To initialize the parameters for the model:
```Python
for(i in range(1, len(layer_dims))):
	parameter['W' + str(i)] = np.random.randn(layers[i], layers[i - 1])) \* 0.01
	parameter['b' + str(i)] = np.random.randn(layers[i], 1) \* 0.01
```
## Coding
1. The model for 2-layer neural network can be summarized as: ***INPUT -> LINEAR -> RELU -> LINEAR -> SIGMOID -> OUTPUT***

The model for L-layer deep neural network can be summarized as: ***[LINEAR -> RELU]  ×  (L-1) -> LINEAR -> SIGMOID***.

2. The ***Deep Learning methodology*** to build the model:
	- 1. Initialize parameters / Define hyperparameters
	- 2. Loop for num_iterations:
		- a. Forward propagation
   		- b. Compute cost function
   		- c. Backward propagation
		- d. Update parameters (using parameters, and grads from backprop) 
	- 3. Use trained parameters to predict labels.

4. **Early stopping** refers to the fact that running the model on fewer iterations gives better accuracy on the test set. We will learn about it in the next course. *Early stopping is a way to prevent overfitting*.

5.  In the next course on "Improving deep neural networks"， we will learn how to obtain even higher accuracy by systematically searching for better hyperparameters (learning_rate, layers_dims, num_iterations, and others).

