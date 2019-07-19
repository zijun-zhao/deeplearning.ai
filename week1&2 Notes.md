# Week 1
## 15July 
1. Deep learning is used to train neural networks 

2. In a neural network, the input layer and the hidden layer(layer in the middle) is densily connected. Every input feature will be connected to the circles in the middle.

3. Examples of sequence data are audio(1D time series and 1D temporal sequence) and language(comes once at a time). 

4. As to structured data, each of the element has a well-definfed meaning. While for unstructured data, features can be pixel value and individual word in a piece of text. 

5. Thanks to deep learning, now a lot of fance applications are created such as speech recognition, image recognation, NLP.

6. Short-term economic value have been created using neural networks such as advertising systems, process giant database, profit recommendation.

7. Increasing the training set size generally does not hurt an algorithm’s performance, and it may help significantly.

8. Increasing the size of a neural network generally does not hurt an algorithm’s performance, and it may help significantly. 

9. RNN (Recurrent Neural Network) is not strictly more powerful than a Convolutional Neural Network (CNN). 

# Week 2
## 16July 
1. This week goes over the basic of neural network programming. Some implemential techniques are very important. For example, in neural network, we want to process a training set of m examples without using an explicit for loop to stepping through all the examples. Another idea is that when organizing the computation of the neural networks, there is a term"forward propogation step" followed by a "backward propagation step/backward pass. So why the computation in learning an NN can be organized in this two processes? This week uses logistic regression to understand those problems.

2. Logistic regression->used for binary classification.

3. A 64x64 pixel image is composed of RGB, to turn the pixel value to feature vector, just unrow them to a long feature vector [64\*64\*3,1]. The total dimension is 64\*64\*3.

4. A useful convention of notation is to take the data(x, y and further other quantities) associated with different training examples and to stack them into different columns(like what we done for both X and Y)

5. When implementing logistic regression, we keep w and b separately. 

6. Loss function is used to measure how good y hat is when the truth is y. Loss function applies to a single training sample.We want it to be convex such that GD works well.

7. Cost function measures how well you are doing on the entire dataset. It is the average over sum of all the loss function applying to each of the training samples. It is the cost of the parameters. During training the neural networks, we try to find w and b that minimizes J overall.

8. Logistic regression can be viewed as a very small neural network.

9. alpha: learning rate, controls how big each step during learning(ways to choose alpha will be taught later)

10. Understanding of calculus helps to understand neural network.

## 18July
1. The computations of a neural network are all organized as a forward path or a forward propagation step in which we compute the output of the neural network, follow by a back complition step which we use to compute gradients/derivatives 

2. Computation graph explains the reason for 1.

3. When coding the derivative of the final output variable towards some variables, in Python just write dJ/dvar as dvar to represent the derivative of the final output variable with respect to the intermediate variables

4. Having explict for loops in the code when running deep learning will make the algorithm less efficiently

5. In order to scale to big dataset, it is important to avoid explict for loops in your code-> vectorization allows you to get rid of the explicit for loop, which becomes even more efficient in deep learning.

6. A lot of scaleable deep learning implementations are done on a GPU(graphics processing unit图像处理单元)。 Both GPU and CPU have parallelization instrutions, sometimes called SIMD(single instruction multiple data)->if you use built-in functions such as np.dot(x,y) or others that do not require you explicitly implementing a for loop, it enables Python numpy to take much better advantage of parallelism.

7. GPU are remakably good at these SIMD calculations

8. Rule of thumb: whenever possible, avoid using explicit for loops

9. Broadcasting turns out to be a technique that Python and numpy allows you to make the code more efficient

## 19July 
1. In python, sum axis=0 means that we want Python to sum vertically,  while the horizontal is axis = 0

2. the reshape function in Python is O(1) computation, so it is a good way to make sure that our matrices are the size that we need.

3. Python can have some internal strange logic, for example, adding a column vector towards a row vector will lead to a matrix as a sum of a row vector and a column vector.

4. np.random.randn(5) will lead to a structure whose shape is (5,). This is a rank 1 array in Python. It is neither a row vector nor a column vector. And as a result, a.T will looks as the same as a, and np.dot(a,a.T) will only give a single value. So Andrew suggested to avoid these rank1-structure whose shape is (n,). Instead, we can write np.random.randn(5,1) to assure that a is a column vector. Then np.dot(a,a.T) while result in a outer product.

5. The result of randn(5) will be [1,2,3,4,5], while randn(5,1) will be [[1,2,3,4,5]], two squares. **[[**  q,w,e,r,t  **]]** is really a 5x1 matrix, while [q,w,e,r,t] is a rank-1 array. 

6. When not sure what is the dimension of the vector, use assert() to make sure something. Like assert(a.shape==(5,1)).

7. When some times you do have a rank1 array, you can write it as a=a.reshape((5,1)).

8. Usually if you are training a learning algorithm you want to make the probabilites large whereas in logistic regression we need to minimize the loss function. So here minimize the loss corresponds to maximizing the log of the probability

9. By minimizing the cost function J(w,b), we are really carring out maxium likelihood estimation with the logistic regresion model's assumption that the training examples are IID

10. In python, a\*b will perform element-wise multiplication, while np.dot(a,b) will perform matirx multiplication. For example, we cannot do a\*b when a is (4,3) but b is (3,2).

