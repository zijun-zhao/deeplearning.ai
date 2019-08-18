## 18 Aug

### Tuning process
1. Example of **Hyperparameters**:
  - ***α***  learning rate
  - **β** momentum term
  - β<sub>1</sub>, β<sub>2</sub>, ε
    - Usually do not tune it
  - *# number of layers*
  - **# number of hidden units**
  - *learning rate decay*
  - **mini-batch size**

2. In deep learning, choose hyperparameters at random, then try out hyperparameters on the randomly chosen set of points. Since it is difficult in advance which hyperparameter is most important to the problem.

3. **Coarse to fine** is a common practice when sampling parameters: zoom in to the sample space and sample densely towards a certain area that works well. 

4. To sum up, the systemtic way to organize the hyperparameter search process is to 
  - Use random sampling and adequate search
     - Allow you to search more efficiently
  - Implement coarse to fine sampling scheme

### Using an approprizte scale to pick hyperparameter
1. Note that sampling at random does not mean sampling uniformly at random over the range of valid values. It is important to pick the appropriate scale on which to explore those paramters

2. **Picking hyperparameters at random**: for number of layers or number of hidden units, maybe it easy to randomly choose a number.

3. **Appropriate scale for hyperparameters**: for learning rate α ranging from 0.001,....1, 90% search will be located between 0.1 to 1, while 10% will be located between 0.0001 to 0.1. In that case, we can search the parameter on a log scale instead of using a linear scale. In that case, log10(1)=0, log10(0.0001)=-4.
```Python
r = -4*np.random.rand()
alpha = 10*exp(r)
```
4. **Hyperparameters for exponentially weighted averages**: for momentum term β. 

  - Suppose β = 0.9,....,0.999. Here choosing 0.9 will take average of 10 days will 0.999 will take average of 1000 days. Transfer this problem of explore the range of (1-β), rangin from 0.1,...,0.0001, just sample r from [-3,-1], then set (1-β)=10exp(r).

  - Why sampling in a linear scale is a bad idea?
    - When β is close to 1, the sensitivity of results you get will change, even with very small changes to β. When β goes from 0.9 to 0.9005, it will average over roughly 10 values, but when β goes from 0.999 to 0.9995 it will average over 1000 and 2000 examples due to the formula 1/(1-β). **When beta is close to 1 it is very sensitive to small changes**. 

  - The whole sampling causes you to sample more densely in the region of β being close to 1, or when (1-β) close to 0. In that case, you can be more efficient in terms of how you distribute the samples to explore the space of possible outcomes more efficiently.

5. Even sampling on a linear scale, if the sum of the scale has been superior, you might still get okay result, especially when using coarse to fine.

### Hyperparameters tuning in practice: Pandas vs. Caviar
1. ***Final tips and tricks for organizing the hyperparameter search process***
  - Re-test hyperparameters occasionally
    - A lot of cross-fertilization among different applications domains exist. 
      - For example, ideas from the computer vision community such as Confonets or ResNets also applies to Speech.
      - Intuitions do get stale. Re-evaluate occasionally.
        - Update servers in your data center may also influce your algorithem.
  - How people go about setting hyperparameters:
    - **Babtsitting one model**: watching performance and patiently nudging the hyperparameters up and down.
        - Usually happens when you have a huge data set but not a lot of computational resources(CPUs and GPUs).
        - Everyday just nudge up and down your parameters and see how the cost function goes. 
        - **Panda**: have very few children. Domains like advertising settings and computer visions will apply this way.
    - **Training many models in parallel**: 
      - Just let several settings run at the same time then plot the cost function in the same plot.
      - **Caviar**: Fish lay over 100million eggs in one mating season
    - Whether Panda or Caviar really depends on **whether you have enough computers to train a lot of models in parallel**

2. ***Barch normalization*** makes your hyperparameter search problem becomes easier.


3. Remember in logistic regression, normalizing the input features can speed up learning. Then for deep learning, when trying to train suitable W<sup>\[3\]</sup> and b<sup>\[3\]</sup>, maybe normalize the activation a<sup>\[2\]</sup> will be meaningful.

4. Batch just normalize a<sup>\[l-1\]</sup> so as to train W<sup>\[l\]</sup> and b<sup>[l]</sup>faster. Although techniquelly we normalize **z<sup>\[l-1\]</sup>** instead of a<sup>[l-1]</sup>. **Batch Norm applies normalization process to both input layer and hidden layers**.
  - In literature, debate exists on wheher normalize a<sup>\[l-1\]</sup> or z<sup>\[l-1\]</sup>.

5. ***Implementing Batch Norm***:
  - Given some itermediate values in the neural network, z<sup>(1)</sup>,...,z<sup>(m)</sup> from some hidden layer z<sup>\[l\](i)</sup>
      - Compuete μ = 1/m\* Σz<sup>\[l\](i)</sup>
      - Compuete σ<sup>2</sup> = 1/m\* Σ(z<sup>\[l\](i)</sup>-μ)
      - z<sup>\[l\](i)</sup><sub>norm</sub> = (z<sup>\[l\](i)</sup>-μ)/(sqrt(σ<sup>2</sup>+ε))
          - Then z<sup>\[l\](i)</sup><sub>norm</sub> has zero mean and variance 1
      - z <sup>~\[l\](i)</sup> = γ\*z<sup>\[l\](i)</sup><sub>norm</sub> + β.
        - Maybe it makes more sense for hidden units to have different distribution.
        - Here γ and β are **learnable parameters** for your model. Algorithms such as gradient descent with momentum or NEsterov, Adam will update γ and β just as you update the weight W.
        - γ and β ar allows you to set the mean and variance for z <sup>~</sup>. When γ = (sqrt(σ<sup>2</sup>+ε)) and β = μ, then z<sup>~\[l\](i)</sup> = z<sup>\[l\](i)</sup>.
          - Sometimes zero mean and 1 variance is not wanted. For instance, if the activation function is Sigmoid function, in order to make better use of the non-linearity, it would be better to have a large variance.


