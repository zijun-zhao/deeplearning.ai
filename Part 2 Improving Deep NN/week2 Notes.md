## 11 Aug
1. 

2. Vectorization allows people to efficiently compute on m examples with no explicit forloop. For instance, X = [x<sup>(1)</sup>, x<sup>(2)</sup>,x<sup>(3)</sup>,...]. It allows people to process all m examples relatively quickly.

3. Since before implementing gradient descent, you need to first process the whole dataset. But if you allow gradient descent to have some progress even before you finish processing the entire giant training set, the process will be speeded up.
	- Split up the training set into a smaller training set, call it Mini-batch. Then X will be composed of X<sup>{1}</sup>, X<sup>{2}</sup>,etc. You will also need to split the training data for Y accordingly.
		- Note that previously we use \*<sup>(i)</sup> to index the i-th training sample, and we use \*<sup>\[l\]</sup>* to index the l-th layer of the neural network. Then now we use the curly brackets \*<sup>{t}</sup> to index the t-th mini barch,
	- When X is (n<sub>x</sub>,m), then X<sup>{t}</sup> will have shape (n<sub>x</sub>,1000) if we separate the original training set every 1000 samples.

4. The name of *Batch gradient descent* refers to that processing the entire batch of training samples all at the same time, while *mini-batch* refers to processing a single mini batch X<sup>{t}</sup>, Y<sup>{t}</sup> at the same time rather than processing the entire training set X.

5. **epoch** means a single pass through the training set.

6. When using *batch gradient descent*, a single pass through the training allows you to take only one gradient descent step.
	- On every iteration you go through the entire training set. The cost function J will decrease on every single iteration.

7. While with *mini-batch gradient descent*, a single pass through the training set allows you to take m/number of sample in a minibatch times gradient descent.
	- Every iteration you are training on different mini batch. The cost function does not decrease on every iteration. Every iteration you process on some X<sup>{t}</sup>, Y<sup>{t}</sup>. The cost function should trend downwards, but it will be a little bit noiser.
		- The oscillation in the cost function J may result from the fact that maybe certain  X<sup>{t}</sup>, Y<sup>{t} is harder and you need some mislabeled examples in it, therefore the cost will be higher  

8. When having a lost training set, mini-batch gradient descent runs much faster than batch gradient descent.

9. ***Choosing the mini-batch size***
	- If mini-ba

Stochastic gradient descent won't ever converge, it will always oscillate and wander around the region of the minimum but it will never just head to the minimum and stay  

10. ***Guidline for choosing the mini-batch size***
	- If small training set(m<=2000)： Use batch gradient descent.
	- Typical mini-batch size: 64, 128, 256, 512,...1024.
	- Make sure minibatch size fit in CPU/GPU memory. Otherwise you will find the performance suddenly falls of  a cliff.

In practice the size of mini-batch is another hyperparameter that you might do a quick search over to try to figure out the most suitable one that is most sufficient in *reducing the cost function J*.

11. Mini-batch gradient descent is used to make your algorightm runs much faster especially when you are training on a large training set. But it turns out that there exist some other algorithms that are even more efficient than gradient descent or mini-batch gradient descent. We will need to know **exponentially weighted averages** to learn those algorithms.

In the example of trying to find out the local average for temparature in London, the general formula can be written as:

V<sub>t</sub> = bataV<sub>t-1</sub> + (1-bata)theta<sub>t</sub>. 	- We can regard V<sub>t</sub> as approximately averaging over 1/(1-beta) day's temparature:
		- When beta = 0.9, ≈10 day's temparature
	 	- When beta = 0.5, only 2 days' temparature is averaged
			More noise will occure, and it will be much more likely to have outliers. But this curve will adapt more quickly to what the temparature changes.
		- When beta = 0.98, ≈ 1/(1-0.98)=50 days' temparature.
			- When beta value is high, the plot will be much smoother because now you are averaging over more days of temparature, therefore the curve becomes less wavy. But on the flip side the curve has now shifted further to the right because you are now averaging over a much larger window of temperatures. And by averaging over a much larger window, the exponentially weighted average formula adapts more slowly when the temparature changes and a bit latency will appear. For example, when beta =0.98, V<sub>t</sub> = 0.98V<sub>t-1</sub> + 0.02theta<sub>t</sub>, only a smaller weight is given to the current value. 

12. In statistics, the name is exponentially weighted moving average, but just call if exponentially weighted average(指数加权平均数). 

13. 


Advantage of the exponentially weighted average formula is that it takes very little memory, the efficiency is better since the storage and momory only need a single row number to compute this exponentially weighted average. Although this it not the most accurate way to compute the average if you were to compute a moving window, you actually can explicitly sum over the last 10 days then divided by 10. But the disadvantage of explicitly keeping all the temparatures around requires more memory and implement more complicated, also has expensive cost.

In the following course, we will see example when we need to compute averages of a lot of variables, this is an efficinet way to do so.
