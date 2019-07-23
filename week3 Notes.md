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

10. A[1](1,1) corresponds to the activation of the first hidden unit on the first training example, while A[1](2,1) corresponds to the activation of the second hidden unit on the first training example.

11. As you scan down the big matrix, it indexes into the hidden unit's number, whereas when you move horizontally, you will go through all the training examples.

12. When you took the different training examples and stack them up into different columns then you will end up z-s stacked as different columns

13. Different layers of neural network are roughly doing the same thing, just over and over

14. In each layer, denote W by: 

,the shape of W is #node x #feature

23July 
1. Since the value of tanh function is between -1 and 1, the mean of the activations in the hidden layer are closer to 0, kinds of has the effect of centering the data, making it easier for the next layer to learn the data, therefore it almost always works better than sigmoid function.

2. One exception using sigmoid function is at the output layer for binary classification.

3. Activation function can be different for different layers, using different number inside the square bracket to denote those functions.

4. One of the downside for both sigmoid function and tanh function is that the slope of the function becomes very small for relative large absolute z(close to 0), which will slow down gradient descent.

5. The rectified linear units
(ReLu) is a good choid

6. Rules of thumb for choosing activation functions:
* When using binary classification, sigmoid function is a good choice, then for all other units using ReLu(rectified liniear unit) is increasingly the default choice of activation function

* When not sure which activation function to choose, use ReLu
* Disvantage of ReLu is that the derivative is equal to 0 when z is negative, but in practice it works fine
* Leakage ReLu usually works better than ReLu but not use as that often
* For both ReLu and leakage Relu, the derivative of the activation function is far from zero, in practice it often learns faster.
* In practice, since we have enough hidden units, so even using ReLu will still have a quite fast learning rate
7. To summarize
* Never use sigmoid function except for the ouput layer if you are doing binary classification
* tanh will be superior than sigmoid
8. You will always having a lot of choices when performing neural networks, like number of hidden units, choices of activation functions, you should choose it according to your applications' idiosyncrasy
