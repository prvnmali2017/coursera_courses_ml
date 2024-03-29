# PyTorch Tutorial: Generative AI Models - EXAMPLE --

In this tutorial, we will learn the basics and advanced concepts of PyTorch and build a simple generative AI application. We will be using the PyTorch library to create a Variational Autoencoder (VAE) for generating new images.

## Table of Contents
1. [Introduction to PyTorch](#introduction-to-pytorch)
2. [Variational Autoencoder (VAE)](#variational-autoencoder-vae)
3. [Building a VAE in PyTorch](#building-a-vae-in-pytorch)
4. [Training the VAE](#training-the-vae)
5. [Generating New Images](#generating-new-images)
6. [Conclusion](#conclusion)

## Introduction to PyTorch
PyTorch is an open-source machine learning library for Python, based on Torch, used for applications such as computer vision and natural language processing. It is primarily developed by Facebook's AI Research lab.

## Variational Autoencoder (VAE)
A Variational Autoencoder (VAE) is a generative model that learns to recreate and generate images. It does this by mapping input images into a lower-dimensional latent space, and then decoding the latent vectors back into images.

## Building a VAE in PyTorch
To build a VAE in PyTorch, we will define two main components: the encoder and the decoder.

### Encoder
The encoder maps input images to a lower-dimensional latent space. It consists of a series of fully connected layers, followed by a ReLU activation function.

```python
class Encoder(nn.Module):
    def __init__(self, latent_dim):
        super(Encoder, self).__init__()

        self.fc1 = nn.Linear(784, 256)
        self.fc2 = nn.Linear(256, 128)
        self.fc3 = nn.Linear(128, latent_dim * 2)

    def forward(self, x):
        x = x.view(-1, 784)
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        mu, logvar = x.split(x.size(1) // 2, dim=1)
        return mu, logvar
```

### Decoder
The decoder maps latent vectors back into images. It consists of a series of fully connected layers, followed by a ReLU activation function (except for the final layer, which uses a sigmoid activation function to ensure output values are in the range [0, 1]).

```python
class Decoder(nn.Module):
    def __init__(self, latent_dim):
        super(Decoder, self).__init__()

        self.fc1 = nn.Linear(latent_dim, 128)
        self.fc2 = nn.Linear(128, 256)
        self.fc3 = nn.Linear(256, 784)
        self.sigmoid = nn.Sigmoid()

    def forward(self, z):
        z = self.fc1(z)
        z = F.relu(z)
        z = self.fc2(z)
        z = F.relu(z)
        z = self.fc3(z)
        return self.sigmoid(z)
```

## Training the VAE
To train the VAE, we will define a loss function that combines the reconstruction loss and the KL divergence loss. The reconstruction loss measures how well the VAE can recreate input images from latent vectors, while the KL divergence loss measures how well the VAE has learned the distribution of the latent space.

```python
def loss_function(recon_x, x, mu, logvar, beta=1):
    recon_loss = nn.MSELoss()(recon_x, x)
    kld_loss = -0.5 * torch.sum(1 + logvar - mu.pow(2) - logvar.exp())
    return recon_loss + beta * kld_loss
```

## Generating New Images
After training the VAE, we can generate new images by sampling latent vectors from a Gaussian distribution and passing them through the decoder.

## Conclusion
In this tutorial, we learned how to build a Variational Autoencoder (VAE) in PyTorch for generating new images. We covered the basics of PyTorch, the architecture of a VAE, and the training process. You can further explore and experiment with different architectures and datasets to create more advanced generative AI models.
