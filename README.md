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

jupyter notebook CNN_MNIST_multilayer_encoder_decoder_with_classifier.ipynb

Purpose

This repository provides a spatial-domain CNN baseline for comparison with optical Fourier neural network approaches.





Single-Pass 4f Fourier CNN Denoiser

Single-Pass 4f Fourier CNN Image Denoiser

Description
This repository implements a Fourier-domain convolutional neural network inspired by a 4f optical system. The model learns a Fourier-plane filter that removes noise from MNIST images.

The implementation acts as a digital simulation of an optical processor that performs filtering in the spatial frequency domain.

Optical Motivation

A classical 4f optical system performs the transformation

U_f(fx,fy) = FourierTransform(u(x,y))

An optical mask placed in the Fourier plane modifies spatial frequencies

U_filtered(fx,fy) = U_f(fx,fy) * H(fx,fy)

The filtered image is reconstructed through

u_out(x,y) = InverseFourierTransform(U_filtered)

This process corresponds directly to frequency-domain filtering.

Neural Network Interpretation

The model learns the Fourier-plane mask H(fx,fy) through training.

Architecture

Input image (28x28x1)

→ OpticalFourierDenoise layer
(FFT → learned Fourier mask → inverse FFT)

Output reconstructed image (28x28x1)

The custom layer internally performs

Fourier transform
Apply learned Fourier mask
Inverse Fourier transform

This makes the model a digital twin of a physical 4f optical processor.

Conceptual Pipeline

Noisy Image

→ Fourier Transform
→ Learned Frequency Filter
→ Inverse Fourier Transform
→ Denoised Image

Evaluation

Reconstruction quality is measured using

PSNR
SSIM

Classification robustness is evaluated by training a classifier on clean MNIST and testing it on
	•	clean digits
	•	noisy digits
	•	denoised digits

This measures how well the Fourier-domain filter preserves digit identity.

Running the Notebook

Requirements

Python
TensorFlow
NumPy
Matplotlib
scikit-learn

Run

jupyter notebook FCNN_4f_single_layer_denoiser_with_classifier_comparison.ipynb

Significance

This model represents a digital implementation of an optical Fourier processor that could be realized using lenses, spatial light modulators, and Fourier-plane masks.








Double-Pass 4f Fourier CNN Denoiser

Double-Pass 4f Optical Fourier CNN Denoiser

Description
This repository implements a cascaded Fourier-domain neural network consisting of two sequential 4f filtering stages. Each stage learns an independent Fourier-plane mask that progressively removes noise from the image.

The architecture simulates multiple optical Fourier processors applied sequentially.

Optical Interpretation

A 4f optical system performs

u_out(x,y) = InverseFourierTransform(FourierTransform(u(x,y)) * H(fx,fy))

In this architecture two 4f systems are applied sequentially.

First stage

u1(x,y) = InverseFourierTransform(FourierTransform(u(x,y)) * H1)

Second stage

u2(x,y) = InverseFourierTransform(FourierTransform(u1(x,y)) * H2)

This produces a cascaded optical filtering system

u(x,y)

→ 4f system 1
→ 4f system 2
→ reconstructed image

Each mask is learned during training.

Model Architecture

Input (28x28x1 image)

→ OpticalFourierDenoise layer (mask H1)
→ OpticalFourierDenoise layer (mask H2)

Output reconstructed image (28x28x1)

Conceptual Pipeline

Noisy image

→ Fourier filtering stage 1
→ intermediate reconstruction
→ Fourier filtering stage 2
→ final denoised image

Evaluation

Reconstruction quality is evaluated using

PSNR
SSIM

Task-based evaluation is performed using a digit classifier trained on clean MNIST. The classifier is tested on
	•	clean digits
	•	noisy digits
	•	denoised digits

This measures how effectively the cascaded Fourier filters restore digit identity.

Running the Notebook

Requirements

Python
TensorFlow
NumPy
Matplotlib
scikit-learn

Run

jupyter notebook FCNN_4f_doublepass_with_classifier_comparison.ipynb

Research Context

This architecture explores stacked Fourier-domain filtering networks inspired by cascaded optical processors in optical neural networks.
