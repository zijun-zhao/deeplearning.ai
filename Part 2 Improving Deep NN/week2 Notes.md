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
	 - = 0.1 θ<sub>100</sub> + 0.1 x 0.9 θ<sub>99</sub> + 0.1 x 0.9<sup>2</sup>θ<sub>98</sub> + 0.1 x 0.9<sup>3</sup>θ<sub>97</sub>...

Then we will have an exponentially decaying function 0.1, 0.1x0.9, 0.1x0.9<sup>2</sup>,... etc. V<sub>100</sub> is calculated by taking the elementwise product between θ<sub>_</sub> and the decaying function. 

15. All the coefficients 0.1, 0.1 x 0.9, 0.1 x 0.9<sup>2</sup>,... add up close to 1, up to a detail called **bisas correction**.

16. In general, (1-ɛ)<sup>(1/ɛ)</sup> ≈ 1/e
	- When β = 0.9, 0.9<sup>10</sup> ≈ 0.35 ≈ 1/e, it takes about 10 days for the height of the decaying function to decay to around 1/e of the peak. Here 10 is obtained from 1/(1-β). Therefore when β = 0.9, the calculation will focus only on the last 1/(1-β) = 10 days temperature, after 10 days, the weight decays to less than 1/e of the weight of the current day. Here ɛ just replaces (1-β).

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

19. **Bias correction** makes the computation of the exponentially weighted average more accurate. Instead of using V<sub>t</sub>, here using V<sub>t</sub>/(1-β<sup>t</sup>), as t becomes large, β<sup>t</sup> will be close to 0, then bias correction will make no difference for a large t. By using bias correction, people will be able to obtain a better estimate of the temperature.

20. If you care about the initial phase's bias, then use bias correction.

21. **Gradient descent with momentum** works faster than the standard gradient descent algorithm, the **baisc idea** is ***to compute an exponentially weighted average of the gradient,then use that gradient to update***.

22. For Gradient descent with momentum,
	- On iteration t
		- Compute dW,db on current mini-batch
		- Compute V<sub>dW</sub> = βV<sub>dW</sub> + (1-β)dW
		- Compute V<sub>db</sub> = βV<sub>db</sub> + (1-β)db
		- Update W as W = W-αV<sub>dW</sub>
		- Update b as b = b-αV<sub>db</sub>
It will smooth out the steps of gradient descent. On the vertical direction(the vertical axis can be regarded as b), it will tend to average out sth. closer to zero, while on horizontal direction, all the derivatives are pointing to the right horizontal direction, the average will still be pretty big. With a few iteration, the result of **gradient descent with momentum** will appear a smaller oscillations in the vertical direction, but are more directed and move quickly to the horizontal direction. 

23. One intuition for **gradient descent with momentum**: when you are trying to minimize the bowl shape function, those derivative terms(dW, db) can be regarded as the acceleration for a ball that you are rolling down hill, and those momentum terms (V<sub>dW</sub>, V<sub>db</sub>) can be recognized as velocity. Becayse if the acceleration, the ball will roll faster and faster, and since β will be smaller than 1, fraction exists therefore the ball will speed up with limit. Gradient descent takes every single step independently of all previous steps, in the **gradient descent with momentum** case the little ball will roll downhill and gain momentum.

24. ***Implementation details***:
	- Initialize V<sub>dW</sub>=0, same shape as W
	- On iteration t:
		- Compute dW, db on the current mini-batch
		- V<sub>dW</sub> = βV<sub>dW</sub> + (1-β)dW
		- V<sub>db</sub> = βV<sub>db</sub> + (1-β)db
		- W = W-αV<sub>dW</sub>, b = b-αV<sub>db</sub>

Here hyperparameters are d and β. β=0.9 will be a common choice which counts the last 10 iterations' gradients. Also, after 

10 iterations the moving average will warmed up and no need to perform bias correction.

25. In some literature, the step becomes
	- V<sub>dW</sub> = βV<sub>dW</sub> + dW
		- V<sub>db</sub> = βV<sub>db</sub> + db
In that case, V<sub>dW</sub> is scaled by a factor of (1-β), therefore the learning rate need to multiple 1/(1-β). Both steps are fun, just the best value of learning rate will be changed accordingly.

26. **RMSprop: root mean square prop** algorithm, can also speed up gradient descent. Square the derivative and take the square root at the end.
	- On iteration t:
		- Compute dW, db on the current mini-batch
		- S<sub>dW</sub> = β<sub>_2</sub>S<sub>dW</sub>= + (1-β<sub>_2</sub>)dW<sup>2</sup>   
			- **here the square is element-wise operation**
			- This step just calculated the exponentially weighted average of the squares of the derivative
		- S<sub>db</sub> = β<sub>_2</sub>S<sub>db</sub>= + (1-β<sub>_2</sub>)db
		- W = W-αdW/(sqrt(S<sub>dW</sub>))
		- b = b-αdb/(sqrt(S<sub>db</sub>))

27. Understanding for RMSprop: When want to slow down the learning at the vertical(b) direction and also speed up the learning on the horizontal direction. We hope that dW will be relatively small, while db will be relatively large. In the example, the function is sloped much more steeply in the vertical direction, db<sup>2</sup> will be relatively large, whereas dW<sup>2</sup> will be smaller, then S<sub>db</sub> will be large, and S<sub>dW</sub> will be small. 

Therefore, a big learning rate alpha can be used without worrying about the diverge in the vertical direction.

In practice, both db and dW will be high-dimension. But the intuition is that when trying to eliminate those oscillation in the direction, you will end up computing a weighyed average for those squares and derivatives S<sub>db</sub> = βS<sub>db</sub>= + (1-β<sub>_2</sub>)db<sup>2</sup>, therefore dumping out the direction haveing oscillations.

28. To make sure the algotirhm will not divide dW and db by zero, ɛ will be added at the nominator to make sure it is nonzero. ɛ can be 10<sup>-8</sup>.

29. RMSprop can speed up gradient descent as well as allowing you yo use a larger learning rate α.

30. Some algorithms proposed by researchers can not be generalized to a wide range of neural networks you might want to train. 

31. **RMSprop** and **Adam** are two algorithms that have stoop up showing it works well in many problems.

32. ***Adam: adapted moment estimation***
	- Initialize V<sub>dW</sub>=0, V<sub>db</sub>=0, S<sub>db</sub>=0
	- On iteration t:
		- Compute dW, db on the current mini-batch
		- V<sub>dW</sub> = β<sub>_1</sub>V<sub>dW</sub>= + (1-β<sub>_1</sub>)dW
		- V<sub>db</sub> = β<sub>_1</sub>V<sub>db</sub>= + (1-β<sub>_1</sub>)db
			- "momentum" β<sub>_1
		- S<sub>dW</sub> = β<sub>_2</sub>S<sub>dW</sub>= + (1-β<sub>_2</sub>)dW<sup>2</sup>   
		- S<sub>db</sub> = β<sub>_2</sub>S<sub>db</sub>= + (1-β<sub>_2</sub>)db<sup>2</sup> 
			- "RMSprop" β<sub>_2
				- Typically, when implementing Adam, bias correction is needed. The V<sub>dW</sub> is a corrected version.
		- V<sup>corrected</sup><sub>dW</sub> = V<sub>dW</sub>(1-β<sub>_1</sub><sup>t</sup>)
		- V<sup>corrected</sup><sub>db</sub> = V<sub>db</sub>(1-β<sub>_1</sub><sup>t</sup>)
		- S<sup>corrected</sup><sub>dW</sub> = S<sub>dW</sub>(1-β<sub>_2</sub><sup>t</sup>)
		- S<sup>corrected</sup><sub>db</sub> = S<sub>db</sub>(1-β<sub>_2</sub><sup>t</sup>)
		- W = W-αV<sup>corrected</sup><sub>dW</sub>/(sqrt(S<sup>corrected</sup><sub>dW</sub> + ɛ))
		- b = b-αV<sup>corrected</sup><sub>db</sub>/(sqrt(S<sup>corrected</sup><sub>db</sub> + ɛ))
33. Hyperparameter in Adam:
	- α: needs to be tun
	- β<sub>_1</sub>: 0.9  (dW, db, the momentum light term)
	- β<sub>_2</sub>： 0.999(dW<sup>2</sup> , db, the momentum light term)
	- ɛ: not matter much.10<sup>-8</sup>.
34. My understanding is that:
	- In gradient descent with momentum
		- When updating W = W-αdW, just replace dW by V<sub>dW</sub>, here V<sub>dW</sub> is a weighted average for dW
	- In RMSprop
		- When calculating S<sub>dW</sub>, dW will be squared
		- When updating W = W-αdW, devide dW and db by sqrt(S<sub>dW</sub>) and sqrt(S<sub>db</sub>) respectively
	- In Adam
		- When updating W = W-αdW, 
			- dW and db is replaced by V<sup>corrected</sup><sub>dW</sub> and αV<sup>corrected</sup><sub>db</sub>
			- Divide the "dW" and "db" by sqrt(S<sup>corrected</sup><sub>dW</sub>) and sqrt(S<sup>corrected</sup><sub>db</sub> ) respectively.
	
35. **Learning rate decay** may help to speed up the algorightm. Intuition for this algorithm: at initial steps you may be able to afford a big step, but when the learning approaches converges, a small learning rate enables people to take small steps.

36. To understand why we need learning rate decay, first imagine a minibatch with size 64, as iterate the steps will be a little noisy, finally wandering around the minimum. **Noise exist in different mini-batches if using fixed value α**. If using a decayed α, you will end up in a tighter region around the minimum.

37. ***Implement rate decay***:
	- alpha = 1/(1+decay_rate\*epoch-num)α<sub>0</sub>. Then as a function of the epoch-num, the learning rate will decrease gradually.
	- Try a lot of decay-rate, α<sub>0</sub>, and find out value that works well.

38. ***Other learning rate decay methods***:
	- Formula
		- α = 0.95<sup>epoch-num</sup>·α<sub>0</sub> : exponentially decay. Here just use some number less than one, 0.95 is an example.
		- α = k/(sqrt(epoch-num))·α<sub>0</sub> or k/(sqrt(t))·α<sub>0</sub>
		- Discrete staircase for α
	- Monual decay
		- Only works if you are training a small number of models

39. Andrew thinks that learning rate decay usually lower down on the list of things he tries. Setting a fixed suitable alpha and get that to be well tuned has a huge impact.

40. Some low-dimentially plots used to guide our intuition, but it turns out that **most points of zero gradients are not local optima, they are just saddle points**.

41. Now that global optima is not problem, **plateaus can really slow down learning**. A plateaus is a region where the derivative is close to 0 for a long time.

42. To sum up the challenge in optimization algorithms might face:
		- **Unlikely to get stuck in a bad local optima**, given that you are training a reasonably large neural network, and the cost function J is also defined in a high dimensional space.
- **Plateaus can make learning slow**. RMSProp or Adam can really help the learning algorithm 




