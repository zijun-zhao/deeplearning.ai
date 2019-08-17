## 11 Aug
This week we will learn optimization algorithm that will enable us to train the neural network much faster. Applied machine learning is a highly empirical process(also a highly iterative process).


1. Deep learning does not work best in a regime of big data because training a huge network on a large dataset is slow. But **Fast optimization algorithm will enable to speed up efficiency**.

2. **Vectorization allows people to efficiently compute on m examples** with no explicit forloop. For instance, X = [x<sup>(1)</sup>, x<sup>(2)</sup>,x<sup>(3)</sup>,...]. It allows people to process all m examples relatively quickly.

3. Since before implementing gradient descent, you need to first process the whole dataset. But if you allow gradient descent to have some progress even before you finish processing the entire giant training set, the process will be speeded up.
	- Split up the training set into a smaller training set, call it **Mini-batch**. Then X will be composed of X<sup>{1}</sup>, X<sup>{2}</sup>, etc. You will also need to split the training data for Y accordingly.
		- Note that previously we use X<sup>(i)</sup> to index the i-th training sample, and we use X<sup>\[l\]</sup> to index the l-th layer of the neural network. Then now we use the curly brackets \*<sup>{t}</sup> to index the t-th mini barch,
	- When X is (n<sub>x</sub> ,m), then X<sup>{t}</sup> will have shape (n<sub>x</sub> ,1000) if we separate the original training set every 1000 samples.

4. The name of *Batch gradient descent* refers to that processing the entire batch of training samples all at the same time, while *mini-batch* refers to processing a single mini-batch X<sup>{t}</sup>, Y<sup>{t}</sup> at the same time rather than processing the entire training set X.

5. **epoch** means a single pass through the training set.

6. When using *batch gradient descent*, a single pass through the entire training set allows you to take only *one* gradient descent *step*.
	- On every iteration, you go through the entire training set. The cost function J will decrease on every single iteration.

7. While with *mini-batch gradient descent*, a single pass through the training set allows you to take m/(number of sample in a minibatch) times gradient descent.
	- Every iteration you are training on a different mini-batch. The cost function does not decrease on every iteration. Every iteration you process on some X<sup>{t}</sup>, Y<sup>{t}</sup>. The cost function should trend downwards, but it will be a little bit noisier.
		- The oscillation in the cost function J may result from the fact that maybe certain X<sup>{t}</sup>, Y<sup>{t}</sup> is harder and you need some mislabeled examples in it, therefore the cost will be higher.  

8. When having a large training set, mini-batch gradient descent runs much faster than batch gradient descent.

9. ***Choosing the mini-batch size***
	- If mini-barch size = m: Batch gradient descent. **One mini-batch**: (X<sup>{1}</sup>, Y<sup>{1}</sup>) = (X,Y)
		- Process a huge training set on every iteration.
		- Take too much time per iteration.
	- If mini-barch size = 1: **Stochastic gradient descent** (X<sup>{t}</sup>, Y<sup>{t}</sup>)=(X<sup>{1}</sup>, Y<sup>{1}</sup>),...,X<sup>{m}</sup>, Y<sup>{m}</sup>. **Every example is its own mini-batch**.
		- Noise can be ameliorated or reduced by using a small learning rate.
		- ***Disadvantage***: lose almost all the speed up from vectorization.
	- In between, minibatch size not too big/small: fastest learning
		- ***Advantage***:
			- Do get a lot of vectorization.
			- Make progress without needing to wait till you process the entire training set.
		
10. Stochastic gradient descent just processes single training example on every iteration. Although most of the time you hit towards the global minimum, sometimes you may hit a wrong direction. Therefore stochastic gradient descent can be extremely noisy. It will always oscillate and wander around the region of the minimum but it will never just head to the minimum and stay. 

On the other hand, although **minibatch size not too big/small** will not be guaranteed to always head towards the minimum. But it **tends to head more consistently in the direction of the minimum than the stochastic descent**.  

11. ***Guideline for choosing the mini-batch size***
	- If small training set(m<=2000)： Use batch gradient descent.
	- Typical mini-batch size: 64, 128, 256, 512,...1024.
	- Make sure minibatch size fit in CPU/GPU memory. Otherwise, you will find the performance suddenly falls of a cliff.

In practice, the size of mini-batch is another hyperparameter that you might do a quick search over to try to figure out the most suitable one that is most sufficient in *reducing the cost function J*.

12. Mini-batch gradient descent is used to make your algorithm runs much faster especially when you are training on a large training set. But it turns out that there exist some other algorithms that are even more efficient than gradient descent or mini-batch gradient descent. We will need to know **exponentially weighted averages** to learn those algorithms.

In the example of trying to find out the local average for temparature in London, the general formula can be written as:
		V<sub>t</sub> = βV<sub>t-1</sub> + (1-β)θ<sub>t</sub>. 	

- We can regard V<sub>t</sub> as approximately averaging over 1/(1-β) day's temparature:
	- When β = 0.9, ≈ 10 day's temparature
	- When β = 0.5, only 2 days' temparature is averaged
		- More noise will occure, and it will be much more likely to have outliers. But this curve will adapt more quickly to what the temparature changes.
	- When β = 0.98, ≈ 1/(1-0.98)=50 days' temparature.
- When **β value** is **high**, the plot will be much **smoother** because now you are averaging over more days of temparature, therefore the curve becomes less wavy. But on the flip side **the curve has now shifted further to the right** because you are now averaging over a much larger window of temperatures. And by averaging over a much larger window, the exponentially weighted average formula **adapts more slowly** when the temparature changes and a bit latency will appear. For example, when β = 0.98, V<sub>t</sub> = 0.98V<sub>t-1</sub> + 0.02θ<sub>t</sub>, only a smaller weight is given to the current value. 

13. In statistics, the name is exponentially weighted moving average, but just call it exponentially weighted average(指数加权平均数). 

14. An example to calculate V<sub>100</sub>:
	 - V<sub>100</sub> = 0.9 V<sub>99</sub> + 0.1 θ<sub>100</sub>
	 - V<sub>99</sub> = 0.9 V<sub>98</sub> + 0.1 θ<sub>99</sub>
	 - V<sub>98</sub> = 0.9 V<sub>97</sub> + 0.1 θ<sub>98</sub>
- Therefore, V<sub>100</sub> 
	 - = 0.9 V<sub>99</sub> + 0.1 θ<sub>100</sub>
	 - = 0.1 θ<sub>100</sub> + 0.9 V<sub>99</sub>
	 - = 0.1 θ<sub>100</sub> + 0.9 (0.9 V<sub>98</sub> + 0.1 θ<sub>99</sub>)
	 - = 0.1 θ<sub>100</sub> + 0.9 (0.1 θ<sub>99</sub> + 0.9 V<sub>98</sub>)
	 - = 0.1 θ<sub>100</sub> + 0.9 (0.1 θ<sub>99</sub> + 0.9 (0.9 V<sub>97</sub> + 0.1 θ<sub>98</sub>))
	 - = 0.1 θ<sub>100</sub> + 0.9 (0.1 θ<sub>99</sub> + 0.9 (0.1 θ<sub>98</sub> + 0.9 V<sub>97</sub>))
	 - = 0.1 θ<sub>100</sub> + 0.1x0.9 θ<sub>99</sub> + 0.1x0.9<sup>2</sup>θ<sub>98</sub> + 0.1x0.9<sup>3</sup>θ<sub>97</sub>...

Then we will have an exponentially decaying function 0.1, 0.1x0.9, 0.1x0.9<sup>2</sup>,... etc. V<sub>100</sub> is calculated by taking the elementwise product between θ<sub>_</sub> and the decaying function. 

15. All the coefficients 0.1, 0.1x0.9, 0.1x0.9<sup>2</sup>,... add up close to 1, up to a detail called **bisas correction**.

16. In general, (1-ɛ)<sup>(1/ɛ)</sup> ≈ 1/e
	- When β = 0.9, 0.9<sup>10</sup> ≈ 0.35 ≈ 1/e, it takes about 10 days for the height of the decaying function to decay to around 1/e of the peak. Here 10 is obtained from 1/(1-β). Therefore when β = 0.9, the calculation will focus only on the last 1/(1-β)=10 days temperature, after 10 days, the weight decays to less than 1/e of the weight of the current day. Here ɛ just replaces (1-β).

17. To implement exponentially weighted averages:
	- V<sub>θ</sub> :=0
	- V<sub>θ</sub> :=  βV<sub>θ</sub> + (1-β)θ<sub>1</sub>
	- V<sub>θ</sub> :=  βV<sub>θ</sub> + (1-β)θ<sub>2</sub>
	- ...
Sometimes using V<sub>θ</sub>, here θ is the subscript to denote that V is computing the exponential average of parameter θ.

18. To summarize, ***implement exponentially weighted averages***:
	- V<sub>θ</sub> :=0
	- Repeat{
		- Get next θ<sub>t</sub>
		- V<sub>θ</sub> :=  βV<sub>θ</sub> + (1-β)θ<sub>2</sub>
	- }
Advantage of the exponentially weighted average formula is that it takes very little memory, the efficiency is better since the storage and memory only need a single row number to compute this exponentially weighted average. Although this it *not the most accurate way* to compute the average if you were to compute a moving window, you actually can explicitly sum over the last 10 days then divided it by 10. But the disadvantage of explicitly keeping all the temparatures around requires more memory and implement more complicated, also has expensive cost.

In the following course, we will see an example when we need to compute averages of a lot of variables, this is an efficinet way to do so.
