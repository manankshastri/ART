# Art generation using Neural Style Transfer

Neural Style Transfer - algorithm created by Gatys et al. (2015) (https://arxiv.org/abs/1508.06576). 

**Here we will**:
- Implement the neural style transfer algorithm
- Generate novel artistic images using the algorithm

## 1. Problem Statement
Neural Style Transfer (NST) is one of the most fun techniques in deep learning. As seen below, it merges two images, namely, a "content" image (C) and a "style" image (S), to create a "generated" image (G). The generated image G combines the "content" of the image C with the "style" of image S. 

For example, an image of the Louvre museum in Paris (content image C), mixed with a painting by Claude Monet, a leader of the impressionist movement (style image S).

<img src="images/louvre_generated.png" style="width:750px;height:200px;">

## 2. Transfer Learning
Neural Style Transfer (NST) uses a previously trained convolutional network, and builds on top of that. The idea of using a network trained on a different task and applying it to a new task is called transfer learning. 

Following the original NST paper (https://arxiv.org/abs/1508.06576), we will use the VGG network. Specifically, we'll use VGG-19, a 19-layer version of the VGG network. This model has already been trained on the very large ImageNet database, and thus has learned to recognize a variety of low level features (at the earlier layers) and high level features (at the deeper layers).

The model is stored in a python dictionary where each variable name is the key and the corresponding value is a tensor containing that variable's value. To run an image through this network, we just have to feed the image to the model. In TensorFlow, we can do so using the [tf.assign](https://www.tensorflow.org/api_docs/python/tf/assign) function. In particular, we will use the assign function like this:  
```python
model["input"].assign(image)
```
This assigns the image as an input to the model. After this, if we want to access the activations of a particular layer, say layer `4_2` when the network is run on this image, we would run a TensorFlow session on the correct tensor `conv4_2`, as follows:  
```python
sess.run(model["conv4_2"])
```

## 3 - Neural Style Transfer 

We will build the NST algorithm in three steps:

- Build the content cost function **J<sub>content</sub>(C,G)**
- Build the style cost function **J<sub>style</sub>(S,G)**
- Put it together to get **J(G) = &#945; J<sub>content</sub>(C,G) + &#946; J<sub>style</sub>(S,G)**

### 3.1 - Computing the content cost
### 3.1.1 - How do you ensure the generated image G matches the content of the image C?

The earlier (shallower) layers of a ConvNet tend to detect lower-level features such as edges and simple textures, and the later (deeper) layers tend to detect higher-level features such as more complex textures as well as object classes. 

We would like the "generated" image G to have similar content as the input image C. Suppose we have chosen some layer's activations to represent the content of an image. In practice, we'll get the most visually pleasing results if we choose a layer in the middle of the network--neither too shallow nor too deep. 

So, suppose we have picked one particular hidden layer to use. Now, set the image C as the input to the pretrained VGG network, and run forward propagation. Let **a<sup>(C)</sup>** be the hidden layer activations in the layer you had chosen. This will be a **n_H x n_W x n_C** tensor. Repeat this process with the image G: Set G as the input, and run forward progation. Let **a<sup>(G)</sup>** be the corresponding hidden layer activation. We will define as the content cost function as:

<img src="images/eq1.PNG" style="width:800px;height:400px;">

Here, **n_H, n_W** and **n_C** are the height, width and number of channels of the hidden layer we have chosen, and appear in a normalization term in the cost. For clarity, note that **a<sup>(C)</sup>** and **a<sup>(G)</sup>** are the volumes corresponding to a hidden layer's activations. In order to compute the cost **J<sub>content</sub>(C,G)**, it might also be convenient to unroll these 3D volumes into a 2D matrix, as shown below. (Technically this unrolling step isn't needed to compute **J<sub>content</sub>**, but it will be good practice for when you do need to carry out a similar operation later for computing the style const **J<sub>style</sub>**.)

<img src="images/NST_LOSS.png" style="width:800px;height:400px;">


### 3.2 - Computing the style cost
### 3.2.1 - Style matrix

The style matrix is also called a "Gram matrix." In linear algebra, the Gram matrix G of a set of vectors **(v<sub>1</sub>,... ,v<sub>n</sub>)** is the matrix of dot products, whose entries are **G<sub>ij</sub> = v<sub>i<sub><sup>T</sup> v<sub>j</sub> = np.dot(v<sub>i</sub>, v<sub>j</sub>)**. In other words, **G<sub>ij</sub>** compares how similar **v_i** is to **v_j**: If they are highly similar, you would expect them to have a large dot product, and thus for **G<sub>ij</sub>** to be large. 
  












## Output (Content Image + Style Image = Generated Image)
<img src="z/11.jpg" style="width:500;height:500px;">
<img src="z/22.jpg" style="width:500;height:500px;">
<img src="z/33.jpg" style="width:500;height:500px;">
<img src="z/44.jpg" style="width:500;height:500px;">
