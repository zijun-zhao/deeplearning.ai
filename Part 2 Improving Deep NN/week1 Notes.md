1. Since from Part1(Neural Networks and Deep Learning) we knew how to implement a neural network, the first week of Part2 will learn the practical aspects of how to make the neural network work well, including hyperparameter tuning, data setup and how to make sure the optimization algorithm runs quickly. 
2. The first week will talk about the cellular machine learning problem, then talk about randomization, then learn how to make sure the implementation of the neural network is correct.

3. Making good choices in how you set up your training ,deelopment and test set will make a huge difference in finding a high performance neural network

4. Applied Machine learning is a highly iterative process
* # layers
* # hidden units
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
