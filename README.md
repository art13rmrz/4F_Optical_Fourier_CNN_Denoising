CNN Image Denoiser

CNN MNIST Image Denoiser

Description
This repository implements a convolutional neural network (CNN) that removes Gaussian noise from MNIST handwritten digit images. The network learns spatial-domain convolution filters that reconstruct clean digits from noisy observations.

This implementation serves as a baseline denoising model used to compare against Fourier-domain optical neural network approaches.

Overview
Image denoising can be formulated as a reconstruction problem where a clean image x is corrupted by noise n to produce a noisy observation y.

y = x + n

The neural network learns a reconstruction function

x_hat = N(y)

that approximates the original clean image.

The CNN performs this task using spatial convolution filters learned through gradient-based optimization.

Model Architecture

Input (28x28x1 image)

→ Convolution layer
→ Max pooling layer
→ Convolution layer
→ Upsampling layer
→ Convolution layer

Output (28x28x1 reconstructed image)

The encoder extracts features from the noisy input while the decoder reconstructs the clean image.

Evaluation

Denoising performance is evaluated using two types of metrics.

Reconstruction metrics
	•	PSNR (Peak Signal-to-Noise Ratio)
	•	SSIM (Structural Similarity)

Task-based evaluation
A digit classifier trained on clean MNIST evaluates
	•	clean images
	•	noisy images
	•	denoised images

This determines whether the denoised images preserve the information necessary for digit recognition.

Running the Notebook

Requirements

Python
TensorFlow
NumPy
Matplotlib
scikit-learn

Run the notebook

jupyter notebook CNN_MNIST_denoiser.ipynb

Purpose

This repository provides a spatial-domain CNN baseline for comparison with optical Fourier neural network approaches.
