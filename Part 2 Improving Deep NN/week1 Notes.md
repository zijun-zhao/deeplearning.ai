## 31July
1. Since from Part1(Neural Networks and Deep Learning) we knew how to implement a neural network, the first week of Part2 will learn the practical aspects of how to make the neural network work well, including hyperparameter tuning, data setup and how to make sure the optimization algorithm runs quickly. 
2. The first week will talk about the cellular machine learning problem, then talk about randomization, then learn how to make sure the implementation of the neural network is correct.

3. Making good choices in how you set up your training ,deelopment and test set will make a huge difference in finding a high performance neural network

4. Applied Machine learning is a highly iterative process
* \# of layers
* \# of hidden units
* activation functions

In pratice, we still go through the process of Idea->Code-> Experiment. Keep iterating to find your best choice

5. Nowadays structured data including advertisements, search(not only internet search, but includes shopping websites), secuity, logistics(物流)

6. The best choices may depend on the amount of data you have, the number of input features, whether you train on GPU or CPU.

7. The workflow is that you train on the training sets, and use the dev set(development set) or the hold-out cross validation set to see which one performs best. After obtaining the final model, we can evaluate on the test set in order to get an unbiased estimate.

8. Previous era of machine learning will chose 70/30.

9. Modern era has the trend that the dev and test set will have a much smaller percentage.

10. Goal of dev set: test different algorithms and see which works better. It need to be big enough to evaluate all the algorithms, no need to be too big

11. **Goal of test set**: given the final classifier, give a unbiased estimate estimate of the performance of your final network(the network you selected)

12. For example, have 1000000 in total, just 10000 for dev and 10000 for test is enough(98% 1% 1% or 99.5% 0.25% 0.25)

13. To sum up, when setting up machine learning problem, we will set it up into a train, dev and test sets. 

14. Another trend for modern machine learning is that people tend to train on **mismatched** train and test distributions.

15. Rule of thumb: assure the test set and dev set come from the same distribution, then the progress in you ML algorithm will be faster.

16. Note that **Not having a test set might be okay(Only dev set)**. In that case, you train on training set, try different model architecture and evaluate them on the dev set. 

17. When just have a training set and a dev set without separate test set, people will call the dev set "test set", but they are actually using the test set as a hold-out cross validation set. This is not a good use of the terminology they overfitting  to the "test set", that "test set" is actually a **trained test set/trained dev set".

18. The setup of train,dev and test set will allow people to integrate more quickly and measure the bias and variance of the algorithm more efficiently so as to improve the algotirhm

19. We talk less of the tradeoff between bias and variance in deep learning.
* High bias: underfitting the data, not doing well on the training set
* High variance: overfitting the data

20. In high dimension, the **train set error** and **dev set error** are used to evaluate the bias and variance.
* By looking at the training set error and the development set error, we would be able to render a diagnosis of whether the algorithm having high variance
	*for example, assume that human level performance gets nearly 0% error, for the cat classification problem:
		* if dev set error is 11% and train set error is 1%, then it has high variance
		* If dev set error is 16 and train set error being 15%, then the algorithm even fit poor for the training set, then it has high bias
		* If dev set error is 15 and train set error being 30%, high bias and high variance

21. The optimal error is called Bayesion error. It turns out of the optimal error is 15%, then the second example above works pretty well.

22. How to analyze bias and variance when no classifier can do very well? Then Bayes error may be higher.

23. Normally, by looking at the training set error, at least you have some idea about how well you are fitting the training data, while turn to the dev set to get a sense of the variance problem.

24. A basic recipe for machine learning: most systematically improve you algorithm's performance  according to variance and bias
* High bias?(training set performance)->the basic problem
	- Bigger network(at least doing well on the training set)
	- Train longer
	- More advanced optimization algorithms
	- (NN architecture search)
* High variance? (dev set performance)
	- More data
	- Regulization
	- (NN architecture search)

25. Note: depending on whether you have high bias or high variance, the set of things you shoud try coculd be quite different:
* Training set is usually used to diagnose if you have a bias or variance problem
	- More data won't help high bias problem
*  Back in the pre-deep learning era, we did not have as many tools to improve bias and variance at the same time. But now:
	- As long as you can keep training a bigger network, it almost always just reduce the bias without necessarily hurting the variance as long as you regularize properly
	- Having more data will reduce the variance without hurting the bias much

26. With the ability to **pick a network** or **get more data**, we now have tools to just drive down just bias or just variance, this is **one of the big reasons why deep learning has been so useful for supervised learning**

27. The main cost of training a neural network is just the computation time

28. Regularization is a useful technique to reduce variance, and there is little bif bias-variance tradeoff when using regularization

29. To address **high variance** problem:
- Get more training data
- Regularization

30. w is usually a high-dimensional parameter 

31. When using L1 regulization, then w will end up being sparse, some people thinks that this helps to compress the model and need less memory, but in practice l1 regularization only helps only a little. People still tend to use L2 regularization

32. In programming, use **lambd**

33. Frombinus norm: sum of square of elements of a matrix

34. Sometimes L2 regularization is called **weight decay**: the first term w is multiplied by a less-than-1 term.

35. Why regularization reduces overfitting? Why shrinking the L2 norm might cause **less overfitting**?
- Intuition: if the lambda is big enough, there will be really incentivized to set the weight metrices W to be reasonbaly closely to 0, thus **zeroing out or at least reducing the impact of a lot of the hidden units
	- Maybe setting the weight to be close to 0 can zeroing out a lot of the impact of hidden units, then the resulting simplified neural network becomes a much smaller one, but stacked very deep, which will change the status much closer to the high bias case, but there will be likely existing a lambda in themiddle
	- Actually we did not completely zeroing out a bunch of hidden units, each of them just have a smaller effect
- For tanh function, if z is small(only taking a small range of parameters), then we just use the linear region of the tanh function
	- When lambda is large, then the weight is small because they are penalized being large into a cos function
	- Since z<sup>\[l\]</sup> = W<sup>\[l\]</sup>a<sup>\[l-1\]</sup>+b<sup>\[l\]</sup>, z will also be relatively small if W is small, then if z in the linear region, every layer will be roughly linear, then the whole network will be linearj just like linear regression, then not able to fit a complicated decision boundary

36. To summarize, if the regularization parameter becomes very large, W will be small, and Z will take on a small range of values, the activation function (if it is tanh) will be relatively linear, the whole neural network will be computing something not too far away from linear function, which also won't overfit

37. **Implementation tip** Remember to update the new defination J when adding the regulirization term.
