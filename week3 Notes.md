## 22July
1. A neutal network can be formed by stacking together a lot of little sigmoid units, whereas previously this node corresponds to several steps of calculation.

2. We use superscript square bracket **[** 1 **]** to refer to quantities associated with the stack of nodes, called a layer. Similarly, use superscript [2] to refer to quantities associated with another layer of the neural network.

3. Square bracket is used to distinguish itself from the superscript round brackets, which are used to refer to individual training examples.

4. To sum up, [x] refers to differnet layers y

5. The term hidden layer refers to the fact that in the training set the true values for these nodes are unknown.

6. In logistic regression, we only have one output layer, therefor the superscript of a is neglected.

7. When we count layers in neural networks, we do not count the input layer.

8. A two-layer neural network only has one hidden layer

9. The horizontal index in the big matrix corresponds to different training examples, while the vertical index corresoonds to different nodes in the neural network

10. A[1](1,1) corresponds to the activation of the first hidden unit on the first training example, while A[1](2,1) corresponds to the activation of the second hidden unit on the first training example.

11. As you scan down the big matrix, it indexes into the hidden unit's number, whereas when you move horizontally, you will go through all the training examples.

12. When you took the different training examples and stack them up into different columns then you will end up z-s stacked as different columns

13. Different layers of neural network are roughly doing the same thing, just over and over
