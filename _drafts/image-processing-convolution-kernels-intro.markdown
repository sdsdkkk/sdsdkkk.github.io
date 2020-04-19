---
layout: content
title: "Image Processing Convolution Kernels Intro"
# date: 2020-04-19 00:00:00
description: A Quick Intro to Convolution Kernels for Image Processing
---

I happen to be exploring image processing using Matlab on Octave (an open-source software compatible with Matlab) on my free time for the past few days. As someone who's relatively inexperienced with image processing, I started by looking at random image kernels on the web before applying it to Matlab environment while I'm testing some random 3x3 image kernels to see how all those image kernels perform against the test images.

The size of a kernel isn't limited to 3x3, but I'm testing only variations of 3x3 kernel for the present.

# Matrix Operation on Image Files with Filter Kernels

Let's suppose we have an image file with the dimension of 5x4 where the image is denoted as the matrix $$A$$. We're aiming to transform the image using the 3x3 kernel $$K$$. The definitions of $$A$$ and $$K$$ is as given below.

$$
  A = \left(
    \begin{matrix}
    3 & 4 & 3 & 4 & 3 \\
    2 & 1 & 2 & 1 & 2 \\
    3 & 2 & 3 & 2 & 3 \\
    4 & 3 & 4 & 3 & 4 \\
    \end{matrix}
  \right)
$$

$$
  K = \left(
    \begin{matrix}
    -1 & -1 & -1 \\
    -1 & 1 & 1 \\
    1 & 1 & 1 \\
    \end{matrix}
  \right)
$$

# Example Kernels

During my search across the Internet, I stumbled upon this simple web-based demo of image kernels by Victor Powell on [Setosa.io](https://setosa.io/ev/image-kernels/). He provides nine example kernels for us to test on sample images, one of them being an identity kernel.

All of the kernels provided by Victor Powell are 3x3 kernels, which is shown below.

## Identity Kernel

The identity kernel wouldn't cause any visible changes to the output image pixels if applied. As the name implies, this is akin to identity in mathematical operations where if an operation is performed between a number with its identity, it will result in the same number. Think $$1 + 0 = 1$$ or $$1 \times 1 = 1$$.

$$
  \left(
    \begin{matrix}
    0 & 0 & 0 \\
    0 & 1 & 0 \\
    0 & 0 & 0 \\
    \end{matrix}
  \right)
$$

## Blur

$$
  \left(
    \begin{matrix}
    0.0625 & 0.125 & 0.0625 \\
    0.125 & 0.25 & 0.125 \\
    0.0625 & 0.125 & 0.0625 \\
    \end{matrix}
  \right)
$$

## Emboss

$$
  \left(
    \begin{matrix}
    -2 & -1 & 0 \\
    -1 & 1 & 1 \\
    0 & 2 & 1 \\
    \end{matrix}
  \right)
$$

## Outline

$$
  \left(
    \begin{matrix}
    -1 & -1 & -1 \\
    -1 & 8 & -1 \\
    -1 & -1 & -1 \\
    \end{matrix}
  \right)
$$

## Sharpen

$$
  \left(
    \begin{matrix}
    0 & -1 & 0 \\
    -1 & 5 & -1 \\
    0 & -1 & 0 \\
    \end{matrix}
  \right)
$$

## Edge Detection

While the previous filters are all named based on the expected visual effects on the image after the kernel is applied, the four Sobel kernels in the list aren't. Sobel is a method for edge detection, for which Victor Powell provided four variations of the kernel on the demo.

### Top Sobel

$$
  \left(
    \begin{matrix}
    1 & 2 & 1 \\
    0 & 0 & 0 \\
    -1 & -2 & -1 \\
    \end{matrix}
  \right)
$$

### Bottom Sobel

$$
  \left(
    \begin{matrix}
    -1 & -2 & -1 \\
    0 & 0 & 0 \\
    1 & 2 & 1 \\
    \end{matrix}
  \right)
$$

### Left Sobel

$$
  \left(
    \begin{matrix}
    1 & 0 & -1 \\
    2 & 0 & -2 \\
    1 & 0 & -1 \\
    \end{matrix}
  \right)
$$

### Right Sobel

$$
  \left(
    \begin{matrix}
    -1 & 0 & 1 \\
    -2 & 0 & 2 \\
    -1 & 0 & 1 \\
    \end{matrix}
  \right)
$$

# References

[Image Kernels Explained Visually by Victor Powell on Setosa.io](https://setosa.io/ev/image-kernels/)
