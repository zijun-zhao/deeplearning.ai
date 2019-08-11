## 31July
1. Since from Part1(Neural Networks and Deep Learning) we knew how to implement a neural network, the first week of Part2 will learn the practical aspects of how to make the neural network work well, including **hyperparameter tuning**, **data setup** and **how to make sure the optimization algorithm runs quickly**. 

2. The first week will talk about the *cellular machine learning problem*, then talk about randomization, then learn how to make sure the implementation of the neural network is correct.

3. Making good choices in how you set up your training, development and test set will make a huge difference in finding a high performance neural network.

4. Applied Machine learning is a highly iterative process
	* \# of layers
	* \# of hidden units
	* activation functions

In pratice, we still go through the process of Idea->Code-> Experiment. Keep iterating to find your best choice

5. Nowadays structured data including advertisements, search(not only internet search, but includes shopping websites), secuity, logistics(物流).

6. The best choices may depend on:
	* The amount of data you have
	* The number of input features
	* Whether you train on GPU or CPU.

7. The workflow is that you train on the training sets, and use the dev set(*development set*) or the *hold-out cross validation set* to see which one performs best. After obtaining the final model, we can evaluate on the test set in order to get an unbiased estimate.

8. Previous era of machine learning will choose 70/30.

9. Modern era has the trend that the dev and test set will have a much smaller percentage.

10. **Goal of dev set**: To test different algorithms and see which works better. It needs to be big enough to evaluate all the algorithms, no need to be too big.

11. **Goal of test set**: Given the final classifier, give an *unbiased estimate* of the performance of your final network(the network you selected).

12. For example, have 1,000,000 in total, just 10,000 for dev and 10,000 for test is enough (98% 1% 1% or 99.5% 0.25% 0.25)

13. ***To sum up***, when setting up machine learning problem, we will set it up into a train, dev and test sets. 

14. Another trend for modern machine learning is that people tend to train on **mismatched** train and test distributions.

15. Rule of thumb: assure the test set and dev set come from the **same distribution**, then the progress in you ML algorithm will be faster.

16. Note that **Not having a test set might be okay(Only dev set)**. In that case, you train on training set, try different model architecture and evaluate them on the dev set. 

17. When just have a training set and a dev set without separate test set, people will call the dev set "test set", but they are actually using the test set as a hold-out cross validation set. This is not a good use of the terminology, actualy they *introduce overfitting to the "test set"*, that "test set" is actually a **trained test set/trained dev set".

18. The setup of train,dev and test set will allow people to integrate more quickly and measure the bias and variance of the algorithm more efficiently so as to improve the algotirhm

19. We talk less of the tradeoff between bias and variance in deep learning.
* **High bias**: Underfitting the data, not doing well on the training set.
* **High variance**: Overfitting the data.

20. In high dimension, the **train set error** and **dev set error** are used to evaluate the bias and variance.
* By looking at the training set error and the development set error, we would be able to render a diagnosis of whether the algorithm having high variance.
	* For example, assume that human level performance gets nearly 0% error, for the cat classification problem:
		* If dev set error is 11% and train set error is 1%, then it has *high variance*.
		* If dev set error is 16 and train set error being 15%, then the algorithm even fit poor for the training set, then it has *high bias*.
		* If dev set error is 15 and train set error being 30%, high bias and high variance.

21. The **optimal error** is called Bayesion error. It turns out THAT if the optimal error is 15%, then the second example above works pretty well.

22. How to analyze bias and variance when no classifier can do very well? When only having a very blurry image, maybe Bayes error becomes higher and there are some details of how this analysis will change.

23. Normally, by looking at the training set error, at least you have some idea about how well you are fitting the training data, which tells you of the bias problem; while when turning to the dev set, you can get a sense of the variance problem.

24. A basic recipe for machine learning: most systematically improve you algorithm's performance according to variance and bias.
	* High bias?(training set performance)->the basic problem
		- Bigger network(at least doing well on the training set)
		- Train longer
		- More advanced optimization algorithms
		- (NN architecture search)
	* High variance? (dev set performance)
		- More data
		- Regulization
		- (NN architecture search)

25. Note: depending on whether you have high bias or high variance, the set of things you shoud try could be quite different:
	* Training set and dev set are usually used to diagnose if you have a bias or variance problem
		- ***More data won't help high bias problem***
	*  Back in the pre-deep learning era, we did not have as many tools to improve bias and variance at the same time. But now:
		- As long as you can keep training a bigger network, it almost always just reduce the bias without necessarily hurting the variance as long as you regularize properly.
		- Having more data will reduce the variance without hurting the bias much.

26. With the ability to **pick a network** or **get more data**, we now have tools to just drive down just bias or just variance, this is ***one of the big reasons why deep learning has been so useful for supervised learning***.

27. The main cost of training a neural network is just the computation time.

28. Regularization is a useful technique to reduce variance, and there is just little bit bias-variance tradeoff when using regularization.

29. To address **high variance** problem:
- Get more training data
- Regularization

30. W is usually a high-dimensional parameter.

31. When using L1 regulization, then W will end up being sparse, some people think that this helps to compress the model and needs less memory, but in practice L1 regularization only helps  a little. People still tend to use L2 regularization.

32. In programming, use **lambd** instead of lambda.

33. Frombinus norm: sum of square of elements of a matrix.

34. Sometimes L2 regularization is called **weight decay**: the first term W is multiplied by a less-than-1 term.

35. Why regularization reduces overfitting? Why shrinking the L2 norm might cause **less overfitting**?
	- Intuition: if the lambda is big enough, there will be really incentivized to set the weight metrices W to be reasonably close to 0, thus **zeroing out or at least reducing the impact of a lot of the hidden units**
		- Maybe setting the weight to be close to 0 can zeroing out a lot of the impact of hidden units, then the resulting simplified neural network becomes a much smaller one, but stacked very deep, which will change the status much closer to the high bias case, but there will be likely exist a lambda in the middle
		- Actually we did not completely zeroing out a bunch of hidden units, each of them just have a smaller effect
	- For tanh function, if |z| is small(only taking a small range of parameters), then we just use the linear region of the tanh function
		- When lambda is large, then the weight is small because they are penalized being large into a cos function.
		- Since z<sup>\[l\]</sup> = W<sup>\[l\]</sup>a<sup>\[l-1\]</sup>+b<sup>\[l\]</sup>, z will also be relatively small if W is small, then if z in the linear region, every layer will be roughly linear, then the whole network will be linear just like linear regression, then not able to fit a complicated decision boundary.

36. To summarize, if the regularization parameter becomes very large, W will be small, and Z will take on a small range of values, the activation function (if it is tanh) will be relatively linear, the whole neural network will be computing something not too far away from linear function, which also won't overfit.

37. **Implementation tip**: Remember to update the new defination J after adding the regulirization term, otherwise it would not decrease monotonically.

## 1August
1. Idea of dropout: randomly knocking our units on the network, train on each example with a much smaller network, therefore end up with a regularized network.

2. Most common way to implement dropout: **inverted dropout**.

3. *keep_prob* is the probability that a given hidden unit will be kept. While *(1-keep_prob)* is the chance to eliminating any hidden units.

4. ***a<sup>\[l\]</sup> /=keep_prob***: 

	- No matter what you set keep_prob to, this inverted dropout technique by dividing the keep_prob is to ensure the **expected value of al remains the same**. By dividing the keep_prob, we just correct the (1-keep_prob) part which we need, so the expected value won't change.

	- At test time, when people trying to evaluate a neural network, the **inverted dropout** technique makes test time easier because people have less of a scaling problem. If not divided the activation vector by keep_prob, the average will become more and more complicated during test time.

6. For different training examples, dropout just zeroing out different hidden unit. In fact, if you make multiple passes through the same training set, then on different pauses through the training set, you should randomly zero out different hidden units.

7. At test time, **not use dropout**, so no need to toss coins at random. The *reason* is that we do not want the output to be random when making predictions, otherwise we will add noise to the predictions. In theory, we can run a prediction process many times with different hidden units randomly dropped out and have it across them, but that is computationally inefficient and will give you roughly the same result.

8. The effect of a<sup>\[l\]</sup> /=keep_prob: **remember the step on the previous line when dividing the keep_prob** is to ensure that even when you do not implement dropout at test time to the scaling, the expected value of those activations do not change, therefore no need to add in an extra scaling parameter at test time.

9. *Intuition of dropout*: 
	* Dropout knocks out units on the network randomly, therefore on every iteration you will work on a smaller neural network, which seems to have a regularization effect.
	* Dropout does not rely on any features, so have to spread out weights, therefore has an effect of **shrinking the squared norm of the weights**:
		* For instance, if a given node has four inputs,  then dropout will eliminate its input randomly, therefore any features will go away randomly, therefore we wont't put too much weight on a particular input, so the unit will be more motivated to spread out the weight to each of those four inputs

10. Difference from drop out to L2 regularization is that *L2 penalty are different on different weight depending on the size of activation being multipled*, but dropout still tends to have a similar effect as L2 regulirization to shrink the weight and helps to prevent overfitting.

11. **Dropout can formally be shown as an adaptive form without a regulization**.

12. keep_prob=1:keep every unit

13. To sum up:
* For layers which you worry more about overfitting, you can set keep_prob to be smaller than others. **Downside** is that it gives you more hyperparameters to search for when using cross-validation
* Option is to apply dropout only on certain layers.

14. *Usually* in practice we *do not implement dropout on the input layer*. Alyhough we can do that.

15. Implementation tips: 
	- In computer vision the input set is so big,dropout is frequently used in CV, but ***to remember***, dropout is a regularization technique and it helps to fit overfitting, *if there is no overfitting problem, we do not need to use dropout*.
		- In CV, we are always not having enough data, therefore overfitting is common.
	- ***Big downside of dropout***: the cost function J is no longer well-defined, it will be hard to check the J plot. Andrew's solvement is to turn off dropout by setting keep_prob=1, then run the code to ensure taht J is monotonically decreasing, and then turn on dropout.

## 3Aug
1. **Data Augmentation**: One way can also be used as a regularization technique: 
	- To help reduce overfitting, one way is to give more data to our algorithm, but it is expensive. There are some inexpensive way to get a larger dataset.
		- For example, when recognizing the cat picture, 
			- Flipping the training example is a good way to enlarge the training set, but it will add redundancy. 
			- Rotate the image and crop it randomly also works
			- Randomly distortion and translation are also okay. 
	By doing the above operation, people can sort of regularize and reduce over fitting.
		- The above methods do not add as many information as adding brand new independent pictures
		- For optical character recognition, we can also bring our data set by taking digits and imposing random rotations and distortions to them.

2.  **Early Stopping**: Another way that can be used as a regularization technique, refers to the process of stopping the training process of your neural network early.

	* When running gradient descent, people usually plot the *training error* or the *cost function*, it will decrease monotonically. We can also plot the dev set error in the same plot. We will find that *the dev set error will first go down for a while and then go up*.

	* What *early stopping* does is to stop at the point of the min dev set error, and take whatever value achieved this dev set error.

	* Reason behind is that: when you haven't run many iterations for your neural network yet, W will be close to zero:
		- With random initialization, then probably W is initialized as small random values. As you iterate, w will get bigger.
		- By **stopping halfway**, you will have a mid-size W. Similar to L2 regularization which picks a neural network with smaller norm for W, hopefully your neural network is **overfitting less**.
	* **Downside** for early stopping:
		- Machine learning includes several steps such as:
			- Optimize your cost function J
				- By choosing an optimizaed algorithm(including Gradient Descent, RMSprop and Adam). But after optimization the cost function J, you also do not want to overfit
			- No overfit
				- Regularization;
				- Getting more data
		- Therefore it is already very complicated to choose among all possible algorithms.
			- For optimizing cost function J, you can only focus on w and b without caring about other parameters.
			- But for not overfitting, you need to **reduce variance**
3. The princinple **ORTHOGONALIZATION** means that we will only think about one task at a time. 
	- For early stopping, you cannot solve the two tasks mentioned above at the same time independently. You are actually trying to solve two problems using the same tool.
	- Rather, if we just implement L2 regularization, we can train the neural network as long as possible and it will *make the search space for hyperparameters easier to decompose*.
		- Downside: need to try various lambda, the searching over possible lambdas are computationally expensive.
	- **Advanatage of early stopping**:
		- By running gradient descent for once, you can try our values of small, middle-size and large W without trying various hyperparameter lambda.

3. Andrew personally favours to use L2 regulirization and try different lambda, assume that he can afford the computation cost.

4. Early stopping will obtain similar result without trying so much lambda.

5. Techniques about **setting up your optimization problem to make your training go quickly**.
	- Normalize the inputs
		- First step: zero out the mean by subtract mean value from the input
		- Normalize the variance by taking each example and divide it by variance to make the variance=1.
	- Remember to use the same miu and sigma square for the test set.

6. Reason for normalization:
	- If NOT notmalize, the cost function J will be a squished out bowl, very enlongated. The learning rate will be small.
	- By normalizing the features, the cost function wil look more symmetric: more round and easier to optimize. Wherever you start, the gradient descent can pretty much *go straight to the minimum*.

7. If the input features came from different scales, it will be imporpant to normalize your features, and *perform normalization never does any harm, so we tend to do it*.

8. **Data vanishing/Exploding gradients**: problems may occur in deep neural network when your *derivative(slope) becomes very big or very small*.
	- This problem was a *huge barrier*  to train deep neural network.

9.  Suppose g(z)=z, then the output y= W<sup>L</sup>W<sup>L-1</sup>W<sup>L-2</sup>...W<sup>3</sup>W<sup>2</sup>W<sup>1</sup>x, while when b<sup>l</sup>=0, W<sup>1</sup>x=z<sup>1</sup>.

If the weight matrix W is just a little bit bigger than the identity matrix, then with a very deep network, the activation can explode. While for a W just a little bit less than identity, the activations will decrease exponentionally. 

10. ***If the way are all a little bigger than the identity matrix, then with a very deep network, the activation will explode exponentionally***. Similaraly, arguments not only suits for the *activation function*, but also applies for the *derivatives*: they will also decrease or increase as a function of number of layers L.

11. Especially if the gradients are exponentially smaller than L, then gradient descent will take tiny little steps to learn.

12. A partial solution which cannot completely solve the data vanishing/exploding problem helps a lot for people to initialize the weights: *choosing a reasonable scaling for how you initialize the weights*. Hopefully, it will make the weights not explode too quicky, and not decay to zero too quickly.

13. In that case, *if the input features of activations are roughly mean 0 and standard variance*, the output z will also in the same range. This step will helps to reduce the vanishing and exploding problem, although do not solve. It just set the value of W to a reasonable number.

14. Common variance to use: W<sup>\[l\]</sup> = np.random.randn(shape)*np.sqrt(2/n<sup>\[l-1\]</sup>)

	- Assume Relu activation functoin, variance of W need to be square root of 2/n<sup>\[l-1\]</sup>.
	- Assume tanh activation functoin, variance of W need to be square root of 1/n<sup>\[l-1\]</sup>. This is called *X<sub>avier</sub>* initialization.
		- X<sub>avier</sub> initialization uses a scaling factor for the weights  W<sup>\[l\]</sup> of sqrt(1./layers_dims[l-1]) , while He initialization would use sqrt(2./layers_dims[l-1])
		- Another version proposed by Yoshua Bengio et.al is the square root of 1/(n<sup>\[l-1\]</sup>+n<sup>\[l\]</sup>)

15. In practice, all the above formulas just give you a starting point for value to use for the variance of the initialization of the weight matrices, the variance parameter can be another thing to tune for hyperparameters.

16. Choosing a reasonable scaling for how you initialize the weights is also a **trick** to make your neural networks trained much quickly.

17. **Gradient checking**: a test to assure that your implementation of back propogation is correct.
	- When using the two-sided difference for gradient checking in back propagation, it runs twice as slow as the one-sided difference.
	- For a nonzero epsilon, we can show the error of approximation :
		- For two-sided: on the order of epsilon square: O(epsilon<sup>2</sup>)
		- For one-sided: on the order of epsilon: O(epsilon)
18. To numerically approximate computations of gradients, we rather use **two-sided difference** since it is more accurate. 

19. By taking the two-sided difference, you can numerically verify whether g(theta) is a correct implementation of the derivative of a functon f.

20. Gradient checking: helpful for finding bugs in the implementation of back propagation.

21. Implement gradient check for back propagation:
	* Take all parameters W<sup>\[1\]</sup>, b<sup>\[1\]</sup>... W<sup>\[L\]</sup>, b<sup>\[L\]</sup> and reshape them into a giant vector data.
		- Reshape all W to vectors and then concate them to obtain a giant vector theta.
		- Cost function will be a function of theta.
	* Obtain dW and db which ordered in the same way, dtheta. Here using the same reshaping and concatenation operation.
		- dtheta and theta will have the same dimension
	* For each i(each component of theta):
		- Compudte dtheta<sub>approx</sub> for every element in the vector theta.
			- Only increase theta<sub>i</sub> by epsilon, and keep everything else the same.
			- dtheta and dtheta<sub>approx</sub> should have the same shape, then need to check whether they are approximately equal to each other .
	* Compute the euclidean distance between the two vectors: the sum of squares of elements of the differences, and then take the sqrt, then normalize it by the lengths of these vectors, divide by ||dtheta<sub>approx</sub>||<sub>2</sub>+||dtheta||<sub>2</sub>.
		- In practice, epcilon can be 10<sup>-7</sup>. In that case, if the ratio is less than or equal to 10<sup>-7</sup>, that will be great, you need to take a careful look if the result 10<sup>-5</sup>.

22. ***Gradient checking implementation notes***:
	* Do not use in training: only to debug
		- Since the two-sided difference runs slow.
	* If algorightm fails grad check, look at components to try to identify bugs.
		- Find which value caused the high difference.
	* Remember regularization.
	* Does not work with dropout
		- Since in every iteration, dropout is randomly eliminating different subsets of the hidden units, there is not an easy way to compute cost function J that dropout is doing gradient descent on.
		- Difficult to use grad check to double check the computation with dropouts
		- Usually just implement grad check without dropout, and then set keep_prob to 1, then turns on dropout and hopes it is correct.
		- You can also fix the pattern of nodes that you dropped and verify the grad check for that pattern of units is code. But Andrew prefers the previous one.
	* Run at random initialization; perhaps again after some training.
		- It might happen that only when w and b becomes large your back propagation is inaccurate. 
		- One thing you can do is to run grad check at random initialization, and train the networks for a while to let W and b far from zero. Then run grad check again after you trained several iterations.

23. This week we learned 
	- How to set up train,dev and test sets
	- What to do for high bias and high variance
	- Apply different forms of regularization
		- L2 regularization
		- Dropout
	- Tracks to speed up the training precess
	- Gradient checking

## Quiz
1. If your Neural Network model seems to have **high bias**, you can
	- Make the Neural Network deeper
	- Increase the number of units in each hideen layer
	- **NO HELP**: getting more training data(this helps the high variance case)

2. **Weight Decay** is a regularization technique (such as L2 regularization) that results in gradient descent shrinking the weights on every iteration.

3. With the *inverted dropout* technique, at test time you *do not apply dropout* (do not randomly eliminate units) and ***do not keep the 1/keep_prob factor*** in the calculations used in training.

4. Increasing the parameter keep_prob from (say) 0.5 to 0.6 will likely cause:
	- Reducing the regularization effect
	- Causing the neural network to **end up with a lower training set error**
		- Reason is that we usually hurt the performance of the training set when implementing regularization

5. To reduce variance(overfitting):
	- L2 regularization
	- Data augmentation
	- Dropout

6. Normalize the inputs x makes the cost function **faster to optimize**.

## Coding
1. ***Initializing all parameters to zeros does not work well since it fails to "break symmetry"***. This means that *every neuron in each layer will learn the same thing*, and you might as well be training a neural network with n<sup>[l]</sup>=1  for every layer, and the network is no more powerful than a linear classifier such as logistic regression. 

2. **What to remember**: 
	- The weights W<sup>[l]</sup> should be initialized randomly to break symmetry. 
	- It is however okay to initialize the biases b<sup>[l]</sup> to zeros. Symmetry is still broken so long as W<sup>[l]</sup> is initialized randomly.
	
3. Following random initialization, each neuron can then proceed to learn a different function of its inputs. 

4.  Use the following for weights:
```Python
np.random.randn(..,..)
```

5. Use the following for biases:
and
```Python 
np.zeros((.., ..)) 
```
6. When initialize weights with **large random values**:
	- The cost starts very high. This is because with large random-valued weights, the last activation (sigmoid) outputs results that are very close to 0 or 1 for some examples, and when it gets that example wrong it incurs a very high loss for that example. Indeed, when log(a<sup>[L]</sup>)=log(0), the loss goes to infinity.
	- Poor initialization can lead to vanishing/exploding gradients, which also slows down the optimization algorithm.
	- If you train that network longer you will see better results, but initializing with overly large random numbers slows down the optimization.
	
7. **In summary**: 
	- Initializing weights to very large random values does not work well. 
	- Hopefully intializing with small random values does better. 
	
7. X<sub>avier</sub> initialization uses a scaling factor for the weights W<sup>\[l\]</sup> of sqrt(1./layers_dims[l-1]) , while He initialization would use sqrt(2./layers_dims[l-1])
	- To implement He initialization, just multiplying np.random.randn(..,..) by sqrt(2./layers_dims[l-1]).
8. Inspiration:
	- Different initializations lead to different results.
	- Random initialization is used to **break symmetry** and make sure different hidden units can learn different things. 
	- Don't intialize to values that are too large. 
	- He initialization works well for networks **with ReLU activations**.
	
9. For a 3-layer neural network, the L2 regularization term should be written as:
```Python
L2_regularization_cost = (1./m*lambd/2)*(np.sum(np.square(W1)) + np.sum(np.square(W2)) + np.sum(np.square(W3)))
```
Here the three np.sum term correspond to the sum over layer l.

All the back propagation steps can be written as below. Here the activation function for the second layer is ReLu. The derivative of g<sup>\[2\]</sup> w.r.t. Z<sup>\[2\]</sup> is 1 when A<sup>\[2\]</sup>>0 or 0 when A<sup>\[2\]</sup><0.
```Python
    dZ3 = A3 - Y
    dW3 = 1./m * np.dot(dZ3, A2.T) + lambd/m * W3
    db3 = 1./m * np.sum(dZ3, axis=1, keepdims = True)
    
    dA2 = np.dot(W3.T, dZ3)
    dZ2 = np.multiply(dA2, np.int64(A2 > 0))
    dW2 = 1./m * np.dot(dZ2, A1.T) + lambd/m * W2
    db2 = 1./m * np.sum(dZ2, axis=1, keepdims = True)
    
    dA1 = np.dot(W2.T, dZ2)
    dZ1 = np.multiply(dA1, np.int64(A1 > 0))
    dW1 = 1./m * np.dot(dZ1, X.T) + lambd/m * W1
    db1 = 1./m * np.sum(dZ1, axis=1, keepdims = True)
 ```
 
 10. L2 regularization makes your decision boundary smoother. If lambda is too large, it is also possible to "oversmooth", resulting in a model with high bias.
 
 13. What is L2-regularization actually doing?: 
 	- L2-regularization relies on the assumption that *a model with small weights is simpler than a model with large weights.* Thus, by penalizing the square values of the weights in the cost function you drive all the weights to smaller values. It becomes too costly for the cost to have large weights! This leads to a smoother model in which the output changes more slowly as the input changes. 
	
14. **What you should remember** from the practice of L2 regularization:
	- ***The implications of L2-regularization on***: 
		- **The cost computation**: 
			- A regularization term is added to the cost 
		- The backpropagation function: 
			- There are **extra terms in the gradients** with respect to weight matrices 
		- Weights end up smaller ("weight decay"): 
			- Weights are pushed to smaller values.
