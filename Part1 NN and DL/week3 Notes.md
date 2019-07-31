## 22July
1. A neutal network can be formed by stacking together a lot of little sigmoid units, whereas previously this node corresponds to several steps of calculation.

2. We use superscript square bracket **[** 1 **]** to refer to quantities associated with the first stack of nodes, called a layer. Similarly, use superscript [2] to refer to quantities associated with second layer of the neural network.

3. Square bracket **[]** is used to distinguish itself from the superscript round brackets **()**, which are used to refer to individual training examples.

4. To sum up, [x] refers to differnet layers x

5. The term **hidden layer** refers to the fact that in the training set the true values for these nodes are unknown.

6. In logistic regression, we only have one output layer, therefor the superscript of a is neglected.

7. **When we count layers in neural networks, we do not count the input layer**.

8. A two-layer neural network only has one hidden layer

9. The horizontal index in the big matrix corresponds to different training examples, while the vertical index corresoonds to different nodes in the neural network

10. a<sup>[1](1)</sup> corresponds to the activation of the first hidden unit on the first training example, while a<sup>[1](2)</sup> corresponds to the activation of the second hidden unit on the first training example.

11. As you scan down the big matrix, it indexes into the hidden unit's number, whereas when you move horizontally, you will go through all the training examples.

12. When you took the different training examples and stack them up into different columns then you will end up z-s stacked as different columns

13. Different layers of neural network are roughly doing the same thing, just over and over

14. The shape of W is #node x #feature

## 23July 
1. Since the **value of tanh function is between -1 and 1**, the mean of the activations in the hidden layer are closer to 0, kinds of has the effect of centering the data, making it easier for the next layer to learn the data, therefore it almost always works better than sigmoid function.

2. One exception using sigmoid function is at the output layer for binary classification.

3. Activation function can be different for different layers, using different number inside the square bracket to denote those functions.

4. One of the downside for both sigmoid function and tanh function is that the slope of the function becomes very small for relative large absolute z(close to 0), which will slow down gradient descent.

5. The rectified linear units (ReLu) is a good choice

6. Rules of thumb for choosing activation functions:
* When using binary classification, sigmoid function is a good choice, then for all other units using ReLu(rectified liniear unit) is increasingly the default choice of activation function
* When not sure which activation function to choose, use ReLu
* Disvantage of ReLu is that the derivative is equal to 0 when z is negative, but in practice it works fine
* Leakage ReLu usually works better than ReLu but not use as that often
* For both ReLu and leakage Relu, the derivative of the activation function is far from zero, in practice it often learns faster.
* In practice, since we have enough hidden units, so even using ReLu will still have a quite fast learning rate

7. To summarize:
  * Never use sigmoid function except for the ouput layer if you are doing binary classification
  * tanh will be superior than sigmoid

8. You will always having a lot of choices when performing neural networks, like number of hidden units, choices of activation functions, you should choose it according to your applications' idiosyncrasy

## 24July
1. When using linear activation function, the output will be linear of the input. No matter how much the layers, you are just computing a linear activation function.

2. If all the activation function at the hidden layers are **linear function** and the output layer uses sigmoid function, then the model has no more expressive than standard logistic regression without any hidden layer.

3. One place to use linear activation function: the output layer. When doing machine learning on regression problem, we may choose linear activation function g(z)=z. But for hidden units we will choose nonlinear activation function such as ReLu, leakage ReLu or tanh.

4. We won't use linear activation function in the hidden layer except for some special circumstances relating to compression 

5. For sigmoid function, the derivative will be close to 0 when z is too big or too small.

6. In neural network, a=g(z), for sigmoid function we can write g'(z)=a(1-a)

7. The derivative for tanh function is 1-tanh^2

8. For ReLu, g(z)=max(0,z), the derivative for a<0 is 0, while the derivative for z>0 is 1. When z=0, you can either set it to 0 or 1. The gradient descent will still work since when z=0, g' becomes the subgradient of the activation function.

9. Similarly, for leakge ReLu, when z=0, you can set it to whatever you want when coding.

10. When training a neural network, it is important to iniialize the parameters randomly

11. For supervised learning, we do not need to take derivative w.r.t. x since x is fixed

12. dw[2] = dz[2]·a[1].T, the transpose is because there is a transpose between W and our individual parameters w

13. For any variable foo and d foo, they have the same dimension

14. One tip when implementing back propagation: if you just make sure that the dimension of your matrices match up, a lot of bug will be eliminated 

15. The shape of parameters:
* W<sup>[1]</sup>:   (n<sup>[1]</sup>, n<sup>[0]</sup>)
* b<sup>[1]</sup>:   (n<sup>[1]</sup>, 1)
* W<sup>[2]</sup>:   (n<sup>[2]</sup>, n<sup>[1]</sup>)
* b<sup>[2]</sup>:   (n<sup>[2]</sup>, 1)

16. Gradient descent for neural networks:
* Parameters: W<sup>[1]</sup>, W<sup>[2]</sup>, b<sup>[1]</sup>, b<sup>[2]</sup> 
  - Repeat
    - Compute derivatives
    - dW<sup>[1]</sup>, dW<sup>[2]</sup>, db<sup>[1]</sup>, db<sup>[2]</sup>
    - Compute predictions y^<sup>[i]</sup>, i = 1,2,...,m
  - Update 
    - W<sup>[1]</sup> - alpha\*dW<sup>[1]</sup>
    - W<sup>[2]</sup> - alpha\*dW<sup>[2]</sup>
    - b<sup>[1]</sup> - alpha\*db<sup>[1]</sup>
    - b<sup>[2]</sup> - alpha\*db<sup>[2]</sup>

## 25July 

1. If you initialize W and b to be all zero, then the activation function will be the same, then if W[2] is also all zero, then after every single iteration of training, the hidden units are still computing exactly the same function

2. It can be proved by induction that if you initialize weights to zero, all hidden units start off computing the same function, and both hidden unit have the same influence on the output unit, then no matter how long you train the unit, both hidden units are still computing exactly the same function.

3. Even you have very big neural big having many hidden units,, if you  initialize all the weights to zero, then all the hidden units are symmetric, they are always computing the  same function.

4. Random initialization is necessary, we usually prefer to initialize with very small random values, because if using tanh or sigmoid, if the weight are too large, calculating the activation function will tend to locate at the flat parts  where the slope is small and the gradient descent would be very slow-> To sum up, big W may lead z to the satuated part of the activation function and result in a slow learning rate. This may not be an issue if you are not using tanh or sigmoid function. But if you are doing binary classification, using sigmoid function may better not use a big initialization.

5. b does not have the symmetry breaking problem, as long as you initialzie W randomly, you start off with a different hidden units computing different things

6. Usually set W[i] =np.random.randn((2,1))\*0.01. When training a deep neural network, how and when to choose a number rather than 0.01 will be taught later

## Summary 
This week learn how to set up a neural network with one hidden layer and how to initialize the parameters to make predictions using forward propagation

## Coding
