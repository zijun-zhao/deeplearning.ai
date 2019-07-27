## 27July

1. Input feature vector x is also the activations of layer 0

2. Andrew didn't think there exist any better way than explicit for loop to implement each layer from 1 to L.

3. Figuring out the dimensions of the various matrices you will be working with is really helpful

4. Main intuition taken away from the facial detection example is that finding simple things like edges and then building them up composing them together to detect more complex things ]

5. Similarly, when building a speech recognition system, you need to know how to visualize speech but if only input an audio clip, the first level of the neural network might detect some low level audio waveform features: tone going up or down, white noise or sibilant sound lights, then by combining those low-level waveform then it can detect the basic units of sound(in linguistics it is called phonemes音位), then maybe able to learn work, then learn sentence.

6. Human brain may also start from learning simple things.

7. Another intuition of why deep neural network works well comes from circuit theory: (Informally) there are functions you can compute with a "small" L-layer deep neural network that shallower networks require exponentially more hidden units to compute. Here small refers to the number of hidden unit in each layer.

8. Andrew thought deep learning is just neural networks with a lot of hidden layers rebranded

9. Given the derivative w.r.t. da[l], how much do i wish a[l-a] may change will show in da[l-1]

10. Concepturally it will be helpful to think of the cache as to store the value of z, but when implementing, you may find that cache is a useful way to get the value of the parameters at W[l] and b[l] into the backward propagation.
