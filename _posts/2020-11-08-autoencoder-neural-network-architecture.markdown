---
layout: post
title: "Autoencoder Neural Network Architecture"
date: 2020-11-08 00:00:00
description: Looking at autoencoders with a bit of PyTorch snippets
tags: MachineLearning SoftwareEngineering
---

An [autoencoder](https://www.jeremyjordan.me/autoencoders/) is a neural network that's composed of two main components, the encoder and the decoder layers. Autoencoders works by compressing the original data into a lower-dimensional vector representation and then reconstructing the compressed data back into the original form.

Since there's a loss of information when compressing the data into its lower-dimensional vector form, there will be some difference between the reconstruction and the original data. This allows an autoencoder to function as a lossy compression engine that can be optimized for a data set with some kind of general pattern contained in it.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw (1.5,4.25) node {Input};
    \draw (5.5,4.25) node {Bottleneck};
    \draw (9.5,4.25) node {Output};

    \draw (0,0) rectangle (3,4);
    \draw (1.5,3.25) circle (0.2in);
    \draw (1.5,2) circle (0.2in);
    \draw (1.5,0.75) circle (0.2in);

    \draw (4,0) rectangle (7,4);
    \draw (5.5,2.5) circle (0.2in);
    \draw (5.5,1.25) circle (0.2in);

    \draw (8,0) rectangle (11,4);
    \draw (9.5,3.25) circle (0.2in);
    \draw (9.5,2) circle (0.2in);
    \draw (9.5,0.75) circle (0.2in);

    \draw [->] (2,3.25) -- (5,2.5);
    \draw [->] (2,3.25) -- (5,1.25);

    \draw [->] (2,2) -- (5,2.5);
    \draw [->] (2,2) -- (5,1.25);

    \draw [->] (2,0.75) -- (5,2.5);
    \draw [->] (2,0.75) -- (5,1.25);
    
    \draw [->] (6,2.5) -- (9,3.25);
    \draw [->] (6,2.5) -- (9,2);
    \draw [->] (6,2.5) -- (9,0.75);
    
    \draw [->] (6,1.25) -- (9,3.25);
    \draw [->] (6,1.25) -- (9,2);
    \draw [->] (6,1.25) -- (9,0.75);
  \end{tikzpicture}
</script>

In the hidden layer, there needs to be a bottleneck layer that has fewer neurons than the layers before and after the bottleneck. The bottleneck's primary function is to reduce the dimensionality of the data in a way that allows the lossy reconstruction of the original data from the information contained in the bottleneck layer, hence compressing it.

# Neural Network Components

For the three-layered autoencoder neural network described in the previous section, we perform the following transformation on the input vector $$I$$ to get the encoded vector $$E$$.

$$
E = W_{encode} I
$$

In order to reconstruct the approximation $$O$$ of the input data back to the original number of dimensions, we perform the following transformation on the encoded vector $$E$$.

$$
O = W_{decode} E
$$

Where $$W_{encode}$$ and $$W_{decode}$$ is the weight matrices for neuron conections in the encoding and decoding process respectively.

On [deep learning](https://www.mathworks.com/discovery/deep-learning.html) implementations, autoencoders have several layers for the encodding and decoding hidden layers. Since the autoencoder's main purpose is still to encode data to a lower-dimensional vector representation and decode it back to the original vector dimension, the bottleneck must have fewer neurons than the other layers in the network.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw (1.5,6.75) node {Input};
    \draw (5.5,6.75) node {Encoder};
    \draw (9.5,6.75) node {Bottleneck};
    \draw (13.5,6.75) node {Decoder};
    \draw (17.5,6.75) node {Output};

    \draw (0,0) rectangle (3,6.5);
    \draw (1.5,5.75) circle (0.2in);
    \draw (1.5,4.5) circle (0.2in);
    \draw (1.5,3.25) circle (0.2in);
    \draw (1.5,2) circle (0.2in);
    \draw (1.5,0.75) circle (0.2in);

    \draw (4,0) rectangle (7,6.5);
    \draw (5.5,5) circle (0.2in);
    \draw (5.5,3.75) circle (0.2in);
    \draw (5.5,2.5) circle (0.2in);
    \draw (5.5,1.25) circle (0.2in);

    \draw (8,0) rectangle (11,6.5);
    \draw (9.5,4.5) circle (0.2in);
    \draw (9.5,3.25) circle (0.2in);
    \draw (9.5,2) circle (0.2in);

    \draw (12,0) rectangle (15,6.5);
    \draw (13.5,5) circle (0.2in);
    \draw (13.5,3.75) circle (0.2in);
    \draw (13.5,2.5) circle (0.2in);
    \draw (13.5,1.25) circle (0.2in);
    
    \draw (16,0) rectangle (19,6.5);
    \draw (17.5,5.75) circle (0.2in);
    \draw (17.5,4.5) circle (0.2in);
    \draw (17.5,3.25) circle (0.2in);
    \draw (17.5,2) circle (0.2in);
    \draw (17.5,0.75) circle (0.2in);

    \draw [->] (2,5.75) -- (5,5);
    \draw [->] (2,5.75) -- (5,3.75);
    \draw [->] (2,5.75) -- (5,2.5);
    \draw [->] (2,5.75) -- (5,1.25);

    \draw [->] (2,4.5) -- (5,5);
    \draw [->] (2,4.5) -- (5,3.75);
    \draw [->] (2,4.5) -- (5,2.5);
    \draw [->] (2,4.5) -- (5,1.25);

    \draw [->] (2,3.25) -- (5,5);
    \draw [->] (2,3.25) -- (5,3.75);
    \draw [->] (2,3.25) -- (5,2.5);
    \draw [->] (2,3.25) -- (5,1.25);

    \draw [->] (2,2) -- (5,5);
    \draw [->] (2,2) -- (5,3.75);
    \draw [->] (2,2) -- (5,2.5);
    \draw [->] (2,2) -- (5,1.25);

    \draw [->] (2,0.75) -- (5,5);
    \draw [->] (2,0.75) -- (5,3.75);
    \draw [->] (2,0.75) -- (5,2.5);
    \draw [->] (2,0.75) -- (5,1.25);

    \draw [->] (6,5) -- (9,4.5);
    \draw [->] (6,5) -- (9,3.25);
    \draw [->] (6,5) -- (9,2);

    \draw [->] (6,3.75) -- (9,4.5);
    \draw [->] (6,3.75) -- (9,3.25);
    \draw [->] (6,3.75) -- (9,2);

    \draw [->] (6,2.5) -- (9,4.5);
    \draw [->] (6,2.5) -- (9,3.25);
    \draw [->] (6,2.5) -- (9,2);

    \draw [->] (6,1.25) -- (9,4.5);
    \draw [->] (6,1.25) -- (9,3.25);
    \draw [->] (6,1.25) -- (9,2);

    \draw [->] (10,4.5) -- (13,5);
    \draw [->] (10,4.5) -- (13,3.75);
    \draw [->] (10,4.5) -- (13,2.5);
    \draw [->] (10,4.5) -- (13,1.25);

    \draw [->] (10,3.25) -- (13,5);
    \draw [->] (10,3.25) -- (13,3.75);
    \draw [->] (10,3.25) -- (13,2.5);
    \draw [->] (10,3.25) -- (13,1.25);

    \draw [->] (10,2) -- (13,5);
    \draw [->] (10,2) -- (13,3.75);
    \draw [->] (10,2) -- (13,2.5);
    \draw [->] (10,2) -- (13,1.25);

    \draw [->] (14,5) -- (17,5.75);
    \draw [->] (14,5) -- (17,4.5);
    \draw [->] (14,5) -- (17,3.25);
    \draw [->] (14,5) -- (17,2);
    \draw [->] (14,5) -- (17,0.75);

    \draw [->] (14,3.75) -- (17,5.75);
    \draw [->] (14,3.75) -- (17,4.5);
    \draw [->] (14,3.75) -- (17,3.25);
    \draw [->] (14,3.75) -- (17,2);
    \draw [->] (14,3.75) -- (17,0.75);

    \draw [->] (14,2.5) -- (17,5.75);
    \draw [->] (14,2.5) -- (17,4.5);
    \draw [->] (14,2.5) -- (17,3.25);
    \draw [->] (14,2.5) -- (17,2);
    \draw [->] (14,2.5) -- (17,0.75);

    \draw [->] (14,1.25) -- (17,5.75);
    \draw [->] (14,1.25) -- (17,4.5);
    \draw [->] (14,1.25) -- (17,3.25);
    \draw [->] (14,1.25) -- (17,2);
    \draw [->] (14,1.25) -- (17,0.75);
  \end{tikzpicture}
</script>

On the deep autoencoder network above, to get the encoded vector $$E$$ of the input vector $$I$$ we need to calculate it as follows.

$$
E = W_{bottleneck} W_{encoder} I
$$

To decode the encoded vector $$E$$ into the output vector $$O$$, we need to perform the following computation.

$$
O = W_{output} W_{decoder} E
$$

In the general implementation of autoencoder network, the layers closer to the bottleneck have fewer neurons than the layers further from the bottleneck. Usually, the input and output layers have the most number of neurons in a layer within the autoencoder network. After the neural network is trained and the weights have been optimized, we can split the neural network into the encoder and decoder networks.

The following is the encoder network.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw (1.5,6.75) node {Input};
    \draw (5.5,6.75) node {Encoder};
    \draw (9.5,6.75) node {Output};

    \draw (0,0) rectangle (3,6.5);
    \draw (1.5,5.75) circle (0.2in);
    \draw (1.5,4.5) circle (0.2in);
    \draw (1.5,3.25) circle (0.2in);
    \draw (1.5,2) circle (0.2in);
    \draw (1.5,0.75) circle (0.2in);

    \draw (4,0) rectangle (7,6.5);
    \draw (5.5,5) circle (0.2in);
    \draw (5.5,3.75) circle (0.2in);
    \draw (5.5,2.5) circle (0.2in);
    \draw (5.5,1.25) circle (0.2in);

    \draw (8,0) rectangle (11,6.5);
    \draw (9.5,4.5) circle (0.2in);
    \draw (9.5,3.25) circle (0.2in);
    \draw (9.5,2) circle (0.2in);

    \draw [->] (2,5.75) -- (5,5);
    \draw [->] (2,5.75) -- (5,3.75);
    \draw [->] (2,5.75) -- (5,2.5);
    \draw [->] (2,5.75) -- (5,1.25);

    \draw [->] (2,4.5) -- (5,5);
    \draw [->] (2,4.5) -- (5,3.75);
    \draw [->] (2,4.5) -- (5,2.5);
    \draw [->] (2,4.5) -- (5,1.25);

    \draw [->] (2,3.25) -- (5,5);
    \draw [->] (2,3.25) -- (5,3.75);
    \draw [->] (2,3.25) -- (5,2.5);
    \draw [->] (2,3.25) -- (5,1.25);

    \draw [->] (2,2) -- (5,5);
    \draw [->] (2,2) -- (5,3.75);
    \draw [->] (2,2) -- (5,2.5);
    \draw [->] (2,2) -- (5,1.25);

    \draw [->] (2,0.75) -- (5,5);
    \draw [->] (2,0.75) -- (5,3.75);
    \draw [->] (2,0.75) -- (5,2.5);
    \draw [->] (2,0.75) -- (5,1.25);

    \draw [->] (6,5) -- (9,4.5);
    \draw [->] (6,5) -- (9,3.25);
    \draw [->] (6,5) -- (9,2);

    \draw [->] (6,3.75) -- (9,4.5);
    \draw [->] (6,3.75) -- (9,3.25);
    \draw [->] (6,3.75) -- (9,2);

    \draw [->] (6,2.5) -- (9,4.5);
    \draw [->] (6,2.5) -- (9,3.25);
    \draw [->] (6,2.5) -- (9,2);

    \draw [->] (6,1.25) -- (9,4.5);
    \draw [->] (6,1.25) -- (9,3.25);
    \draw [->] (6,1.25) -- (9,2);
  \end{tikzpicture}
</script>

And the following is the decoder network.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw (9.5,6.75) node {Input};
    \draw (13.5,6.75) node {Decoder};
    \draw (17.5,6.75) node {Output};

    \draw (8,0) rectangle (11,6.5);
    \draw (9.5,4.5) circle (0.2in);
    \draw (9.5,3.25) circle (0.2in);
    \draw (9.5,2) circle (0.2in);

    \draw (12,0) rectangle (15,6.5);
    \draw (13.5,5) circle (0.2in);
    \draw (13.5,3.75) circle (0.2in);
    \draw (13.5,2.5) circle (0.2in);
    \draw (13.5,1.25) circle (0.2in);
    
    \draw (16,0) rectangle (19,6.5);
    \draw (17.5,5.75) circle (0.2in);
    \draw (17.5,4.5) circle (0.2in);
    \draw (17.5,3.25) circle (0.2in);
    \draw (17.5,2) circle (0.2in);
    \draw (17.5,0.75) circle (0.2in);

    \draw [->] (10,4.5) -- (13,5);
    \draw [->] (10,4.5) -- (13,3.75);
    \draw [->] (10,4.5) -- (13,2.5);
    \draw [->] (10,4.5) -- (13,1.25);

    \draw [->] (10,3.25) -- (13,5);
    \draw [->] (10,3.25) -- (13,3.75);
    \draw [->] (10,3.25) -- (13,2.5);
    \draw [->] (10,3.25) -- (13,1.25);

    \draw [->] (10,2) -- (13,5);
    \draw [->] (10,2) -- (13,3.75);
    \draw [->] (10,2) -- (13,2.5);
    \draw [->] (10,2) -- (13,1.25);

    \draw [->] (14,5) -- (17,5.75);
    \draw [->] (14,5) -- (17,4.5);
    \draw [->] (14,5) -- (17,3.25);
    \draw [->] (14,5) -- (17,2);
    \draw [->] (14,5) -- (17,0.75);

    \draw [->] (14,3.75) -- (17,5.75);
    \draw [->] (14,3.75) -- (17,4.5);
    \draw [->] (14,3.75) -- (17,3.25);
    \draw [->] (14,3.75) -- (17,2);
    \draw [->] (14,3.75) -- (17,0.75);

    \draw [->] (14,2.5) -- (17,5.75);
    \draw [->] (14,2.5) -- (17,4.5);
    \draw [->] (14,2.5) -- (17,3.25);
    \draw [->] (14,2.5) -- (17,2);
    \draw [->] (14,2.5) -- (17,0.75);

    \draw [->] (14,1.25) -- (17,5.75);
    \draw [->] (14,1.25) -- (17,4.5);
    \draw [->] (14,1.25) -- (17,3.25);
    \draw [->] (14,1.25) -- (17,2);
    \draw [->] (14,1.25) -- (17,0.75);
  \end{tikzpicture}
</script>

# Sample Implementation

The following is a sample implementation of a deep autoencoder network using [PyTorch](https://pytorch.org/).

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

class Autoencoder(nn.Module):
    def __init__(self):
        super(Autoencoder, self).__init__()
        
        dimension = 28*28
        self.encoder = nn.Linear(dimension, int(dimension/2))
        self.bottleneck = nn.Linear(int(dimension/2), int(dimension/4))
        self.decoder = nn.Linear(int(dimension/4), int(dimension/2))
        self.output = nn.Linear(int(dimension/2), dimension)
        
    def forward(self, x):
        x = self.encoder(x)
        x = F.relu(x)
        x = self.bottleneck(x)
        x = F.relu(x)
        
        x = self.decoder(x)
        x = F.relu(x)
        x = self.output(x)
        x = F.relu(x)

        return x
```

The code snippet implements the following deep autoencoder network from the previous section.



<script type="text/tikz">
  \begin{tikzpicture}
    \draw (1.5,6.75) node {Input};
    \draw (5.5,6.75) node {Encoder};
    \draw (9.5,6.75) node {Bottleneck};
    \draw (13.5,6.75) node {Decoder};
    \draw (17.5,6.75) node {Output};

    \draw (0,0) rectangle (3,6.5);
    \draw (1.5,5.75) circle (0.2in);
    \draw (1.5,4.5) circle (0.2in);
    \draw (1.5,3.25) circle (0.2in);
    \draw (1.5,2) circle (0.2in);
    \draw (1.5,0.75) circle (0.2in);

    \draw (4,0) rectangle (7,6.5);
    \draw (5.5,5) circle (0.2in);
    \draw (5.5,3.75) circle (0.2in);
    \draw (5.5,2.5) circle (0.2in);
    \draw (5.5,1.25) circle (0.2in);

    \draw (8,0) rectangle (11,6.5);
    \draw (9.5,4.5) circle (0.2in);
    \draw (9.5,3.25) circle (0.2in);
    \draw (9.5,2) circle (0.2in);

    \draw (12,0) rectangle (15,6.5);
    \draw (13.5,5) circle (0.2in);
    \draw (13.5,3.75) circle (0.2in);
    \draw (13.5,2.5) circle (0.2in);
    \draw (13.5,1.25) circle (0.2in);
    
    \draw (16,0) rectangle (19,6.5);
    \draw (17.5,5.75) circle (0.2in);
    \draw (17.5,4.5) circle (0.2in);
    \draw (17.5,3.25) circle (0.2in);
    \draw (17.5,2) circle (0.2in);
    \draw (17.5,0.75) circle (0.2in);

    \draw [->] (2,5.75) -- (5,5);
    \draw [->] (2,5.75) -- (5,3.75);
    \draw [->] (2,5.75) -- (5,2.5);
    \draw [->] (2,5.75) -- (5,1.25);

    \draw [->] (2,4.5) -- (5,5);
    \draw [->] (2,4.5) -- (5,3.75);
    \draw [->] (2,4.5) -- (5,2.5);
    \draw [->] (2,4.5) -- (5,1.25);

    \draw [->] (2,3.25) -- (5,5);
    \draw [->] (2,3.25) -- (5,3.75);
    \draw [->] (2,3.25) -- (5,2.5);
    \draw [->] (2,3.25) -- (5,1.25);

    \draw [->] (2,2) -- (5,5);
    \draw [->] (2,2) -- (5,3.75);
    \draw [->] (2,2) -- (5,2.5);
    \draw [->] (2,2) -- (5,1.25);

    \draw [->] (2,0.75) -- (5,5);
    \draw [->] (2,0.75) -- (5,3.75);
    \draw [->] (2,0.75) -- (5,2.5);
    \draw [->] (2,0.75) -- (5,1.25);

    \draw [->] (6,5) -- (9,4.5);
    \draw [->] (6,5) -- (9,3.25);
    \draw [->] (6,5) -- (9,2);

    \draw [->] (6,3.75) -- (9,4.5);
    \draw [->] (6,3.75) -- (9,3.25);
    \draw [->] (6,3.75) -- (9,2);

    \draw [->] (6,2.5) -- (9,4.5);
    \draw [->] (6,2.5) -- (9,3.25);
    \draw [->] (6,2.5) -- (9,2);

    \draw [->] (6,1.25) -- (9,4.5);
    \draw [->] (6,1.25) -- (9,3.25);
    \draw [->] (6,1.25) -- (9,2);

    \draw [->] (10,4.5) -- (13,5);
    \draw [->] (10,4.5) -- (13,3.75);
    \draw [->] (10,4.5) -- (13,2.5);
    \draw [->] (10,4.5) -- (13,1.25);

    \draw [->] (10,3.25) -- (13,5);
    \draw [->] (10,3.25) -- (13,3.75);
    \draw [->] (10,3.25) -- (13,2.5);
    \draw [->] (10,3.25) -- (13,1.25);

    \draw [->] (10,2) -- (13,5);
    \draw [->] (10,2) -- (13,3.75);
    \draw [->] (10,2) -- (13,2.5);
    \draw [->] (10,2) -- (13,1.25);

    \draw [->] (14,5) -- (17,5.75);
    \draw [->] (14,5) -- (17,4.5);
    \draw [->] (14,5) -- (17,3.25);
    \draw [->] (14,5) -- (17,2);
    \draw [->] (14,5) -- (17,0.75);

    \draw [->] (14,3.75) -- (17,5.75);
    \draw [->] (14,3.75) -- (17,4.5);
    \draw [->] (14,3.75) -- (17,3.25);
    \draw [->] (14,3.75) -- (17,2);
    \draw [->] (14,3.75) -- (17,0.75);

    \draw [->] (14,2.5) -- (17,5.75);
    \draw [->] (14,2.5) -- (17,4.5);
    \draw [->] (14,2.5) -- (17,3.25);
    \draw [->] (14,2.5) -- (17,2);
    \draw [->] (14,2.5) -- (17,0.75);

    \draw [->] (14,1.25) -- (17,5.75);
    \draw [->] (14,1.25) -- (17,4.5);
    \draw [->] (14,1.25) -- (17,3.25);
    \draw [->] (14,1.25) -- (17,2);
    \draw [->] (14,1.25) -- (17,0.75);
  \end{tikzpicture}
</script>

What we didn't discuss in the previous section is the [activation function](https://en.wikipedia.org/wiki/Activation_function). Each layers in the code snippet is using the [ReLU](https://en.wikipedia.org/wiki/Rectifier_(neural_networks)) function for activation.

The encoder layer in the snippet takes in an input vector with the size of $$28 \times 28 = 784$$ and generates a vector with the size of $$\frac{784}{2} = 392$$ as the output, which will be fed to the bottleneck layer which is going to reduce it into a vector with the size of only $$\frac{784}{4} = 196$$ as the encoded data.

The decoder layer will take the value from the bottleneck as its input and then pass it to the output layer to reconstruct it back to its original size.

The following snippet shows how to train the autoencoder using the MNIST handwritten digits dataset.

```python
import torchvision
import torchvision.datasets as datasets
import torchvision.transforms as transforms
import torch.optim as optim

mnist_trainset = datasets.MNIST(root='./data', train=True, download=True, transform=None)
mnist_testset = datasets.MNIST(root='./data', train=False, download=True, transform=None)

autoencoder = Autoencoder()

criterion = nn.MSELoss()
optimizer = optim.Adam(autoencoder.parameters(), lr=0.001)
trans = transforms.ToTensor()
epoch = 10

for epoch_count in range(epoch):
    count = 0
    for x, y in mnist_trainset:
        optimizer.zero_grad()
        in_v = trans(x).reshape((28*28))
        out_v = autoencoder(in_v)
        loss = criterion(out_v, in_v)
        loss.backward()
        optimizer.step()
        count = count + 1
        # Break after 100 sample data for testing
        if count > 100:
            break
```

# Extra Notes

[This video](https://www.youtube.com/watch?v=LeeBnlZLTBs) shows several applications of autoencoder in real life, including denoising, image restoration, recommendation systems, and [deepfake](https://en.wikipedia.org/wiki/Deepfake).

# References

[Introduction to autoencoders.](https://www.jeremyjordan.me/autoencoders/)

[What Is Deep Learning?](https://www.mathworks.com/discovery/deep-learning.html)

[PyTorch](https://pytorch.org/)

[Activation function](https://en.wikipedia.org/wiki/Activation_function)

[Rectifier (neural networks)](https://en.wikipedia.org/wiki/Rectifier_(neural_networks))

[Fun With Autoencoders (Deep Fakes, Recommendation Engines & more)](https://www.youtube.com/watch?v=LeeBnlZLTBs)

[Deepfake](https://en.wikipedia.org/wiki/Deepfake)
