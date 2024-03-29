---
layout: post
title:  "Diffusion Models"
date:   2024-01-27 11:53:18 +0530
categories: AI
---

## Table of Contents
1. [Denoising Probabilistic Diffusion Models](#Denoising-Probabilistic-Diffusion-Models)
1. [Similarities to Variational Autoencoders](#similarities-to-variational-autoencoders)
1. [Latent Diffusion Models](#latent-diffusion-models)


Denoising Probabilistic Diffusion Models
--------------------------------------------------------

#### **The Forward Diffusion Process: Mapping Images to Gaussian Noise**

Lets start with adding a bit of Gaussian noise to an image. By repeating this process for a sufficient number of steps, we can transform any image into a sample from a pure Gaussian distribution. 

Consider an image, which we'll denote as $$X_0$$. If we add Gaussian noise, denoted as $$e_0$$, we obtain a slightly noisier image, $$X_1$$. Continuing this process for $$T$$ time steps leads us through a sequence:

$$ X_0 \rightarrow X_1 \rightarrow X_2 \rightarrow \cdots \rightarrow X_T $$

At each step, we add noise $$e_t$$ to transform $$X_t$$ into $$X_{t+1}$$. As the number of time steps increases, $$X_T$$ converges to a sample from a pure Gaussian distribution, effectively becoming indistinguishable from random noise. This transformation is a Markov process since each image $$X_{t-1}$$ depends only on its immediate predecessor $$X_t$$ and the time step $$t$$.

Mathematically, this process is defined as:

$$ Q(X_t | X_{t-1}) = X_{t-1} + N(u_t, s_t) $$
$$ = X_{t-1} + u_t + e \cdot s_t \,\,\, \text{where} \,\,\, e \sim N(0, I) $$

Here, $$N(u_t, s_t)$$ represents the Gaussian noise added at each step, with each pixel receiving noise sampled from this distribution.

#### **The Reverse Process: Denoising to Generate Images**

The ultimate goal, however, is to reverse this process. Starting with a sample from the Gaussian distribution (pure noise), we aim to denoise it over multiple time steps to reconstruct an image $$X_0$$. This is denoted as:

$$ X_T \rightarrow X_{T-1} \rightarrow X_{T-2} \rightarrow \cdots \rightarrow X_0 $$

To achieve this, we focus on solving $$P(X_{t-1} \| X_t)$$ for any given time step. Assuming the noise added at each step is small and the number of steps is large, we can approximate the reverse process's probability space $$P(X_{t-1} \| X_t)$$ as a Gaussian distribution.

This is where a neural network comes into play, parameterizing the reverse process:

$$ P(X_{t-1} | X_t, t) = N(\mu_0, \sigma_0) $$

Here, $$\mu_0$$ and $$\sigma_0$$ are the outputs of the neural network, defining the mean and standard deviation of the Gaussian distribution from which $$X_{t-1}$$ is sampled. The network takes the noisy image $$X_t$$ and the time step $$t$$ as inputs.

Alternatively, we can view this as predicting the difference in images (the added noise):

$$ e_t = X_t - X_{t-1} $$
$$ P(e_t | X_t, t) = X_t + N(\mu_{t_0}, \sigma_{t_0}) $$

The neural network aims to account for the noise to be subtracted from $$X_t$$ to obtain $$X_{t-1}$$.

#### **Key Takeaways and Design Considerations**

The essence of this model is breaking down a complex image distribution into many time steps, such that the conditional distribution of $$X_{t-1} \\| X_t$$ can be modeled by a simple distribution, achievable by a neural network.

Remember, each step is defined by distributions: noise is added according to a schedule in the forward process, and in the reverse process, we subtract noise sampled from a Gaussian distribution to denoise.

#### **Noise Schedules and Distribution Choices**

While the above process is quite general, we can simplify our approach by making specific choices. We aim to arrive at a pure Gaussian distribution after a large number of time steps. Thus, we fix the final distribution as $$N(0, I)$$, with a mean of 0 and a variance of 1, implying that each pixel is independently sampled from $$N(0, 1)$$.

We introduce a noise schedule, $$b_0, b_1, b_2, \ldots, b_t$$, to regulate the amount of noise added at each step, imposing the condition $$0 < b_0 < b_1 < b_2 \ldots < 1.0$$.



Let's incorporate and clarify the technical details provided in your outline:

### **Transition Property and Direct Relation to Initial Image**

A critical aspect of the forward process in probabilistic diffusion models is the transition probability, which can be represented as:

$$ q(x_t | x_{t-1}) := \mathcal{N} \left(x_t; \sqrt{1 - \beta_t} x_{t-1}, \beta_t I \right) $$

This formulation allows us to express $$ x_t $$ directly in terms of the original image $$ x_0 $$, bypassing the need to sample intermediate states $$ x_1, x_2, \ldots, x_t $$. Using the notations $$ \alpha_t := 1 - \beta_t $$ and $$ \bar{\alpha}_t := \prod_{s=1}^{t} \alpha_s $$, the relationship is given by:

$$ q(x_t | x_0) = \mathcal{N} \left(x_t; \sqrt{\bar{\alpha}_t} x_0, (1 - \bar{\alpha}_t)I \right) $$

This direct relationship simplifies the computation and enhances the model's efficiency.

### **Reparameterization Trick**

A useful reparameterization trick in this context is:

$$ \mathcal{N}(u, s) = u + e \cdot s, \,\,\, \text{where} \,\,\, e \sim \mathcal{N}(0, I) $$

Hence, $$ x_t $$ can be expressed as a function of $$ x_0 $$ and noise $$ e $$.

### **Simplified Loss Function**

The loss function plays a crucial role in training the model. It can be simplified as follows:

$$ L_{\text{simple}}(\theta) := \mathbb{E}_{t, x_0, \epsilon} \left[ \|\epsilon - \epsilon_\theta(\sqrt{\bar{\alpha}_t} x_0 + \sqrt{1 - \bar{\alpha}_t} \epsilon, t)\|^2 \right] $$

In this formulation, the model aims to predict the noise $$ \epsilon $$ from $$ x_t $$ and time step $$ t $$.

### **Reverse Process: Predicting Noise and Reconstructing Images**

During the reverse process, the model predicts the noise and reconstructs the previous image state. For a given $$ x_t $$, $$ t $$, the model predicts $$ \epsilon $$ and reconstructs $$ x_{t-1} $$ as:

$$ x_{t-1} = \frac{1}{\sqrt{\alpha_t}} \left( x_t - \frac{\sqrt{1 - \alpha_t}}{1 - \bar{\alpha}_t} \epsilon_\theta(x_t, t) \right) + \sigma_t z $$

Here, $$ \epsilon_\theta $$ is the predicted noise by the model, $$ \sigma_t $$ is a scaling factor, and $$ z $$ is sampled from $$ \mathcal{N}(0, I) $$.

### **Conclusion**

In summary, the model leverages the Markovian nature of the forward process and the reverse process's ability to denoise and reconstruct images. By breaking down the complex image distribution into smaller, manageable steps, and using a neural network to approximate the transition probabilities, denoising probabilistic diffusion models provide a powerful framework for generating high-quality images from noise. The use of a simplified loss function and efficient computation methods like the reparameterization trick further enhance the model's effectiveness.

Similarities to Variational Autoencoders
--------------------------------------------------------


Variational Autoencoders (VAEs) and Denoising Probabilistic Diffusion Models are both generative models in machine learning, but they operate on different principles. Despite their differences, there are some conceptual similarities. Let's explore these similarities along with their respective formulas:

### Variational Autoencoders (VAEs)

1. **Model Structure:** VAEs consist of an encoder and a decoder. The encoder maps input data to a latent space, and the decoder reconstructs the input data from this latent space.

   - **Encoder:** Maps input $$ x $$ to a latent representation $$ z $$:
     $$ q(z | x) = \mathcal{N}(z; \mu(x), \sigma(x)^2 I) $$
   - **Decoder:** Reconstructs $$ x $$ from $$ z $$:
     $$ p(x | z) = \mathcal{N}(x; f(z), \sigma_x^2 I) $$

2. **Objective Function:** The training involves maximizing the Evidence Lower Bound (ELBO):
   $$ \text{ELBO} = \mathbb{E}_{q(z|x)}[\log p(x|z)] - \text{KL}[q(z|x) \| p(z)] $$
   This includes a reconstruction term and a regularization term (KL divergence).

### Denoising Probabilistic Diffusion Models

1. **Model Structure:** These models gradually add noise to data and then learn to reverse this process.

   - **Forward Process:** Gradually adds noise to data $$ x_0 $$:
     $$ q(x_t | x_{t-1}) = \mathcal{N}(x_t; \sqrt{1 - \beta_t} x_{t-1}, \beta_t I) $$
   - **Reverse Process:** Learns to denoise the data:
     $$ p(x_{t-1} | x_t) = \mathcal{N}(x_{t-1}; \mu_{\theta}(x_t, t), \sigma_{\theta}(x_t, t)^2 I) $$

2. **Objective Function:** The loss function typically involves predicting the noise added to the data:
   $$ L(\theta) = \mathbb{E}_{t, x_0, \epsilon}[\|\epsilon - \epsilon_{\theta}(x_t, t)\|^2] $$

### Similarities

1. **Probabilistic Foundations:** Both models are rooted in probabilistic frameworks, using Gaussian distributions to model the generation and transformation of data.

2. **Latent Space Representation:** While VAEs explicitly encode data into a latent space, diffusion models implicitly use a latent space representation as they transform data into noise and back.

3. **Reconstruction Objective:** Both models have an objective function that involves reconstruction. VAEs directly aim to reconstruct input data, whereas diffusion models focus on reconstructing data from noise.

4. **Regularization and Noise Modeling:** VAEs regularize the latent space using the KL divergence term, ensuring a structured latent space. Diffusion models implicitly regularize the data generation process by modeling the noise added at each step.

5. **Learning Complex Distributions:** Both models are adept at learning complex data distributions, with VAEs doing so in the latent space and diffusion models through the noise addition and removal process.

### Conclusion

While Variational Autoencoders and Denoising Probabilistic Diffusion Models have distinct mechanisms and objectives, they share similarities in their probabilistic approach, latent space conceptualization, and focus on reconstructing complex data distributions. These similarities highlight the rich, varied approaches in the field of generative modeling.




Latent Diffusion Models
--------------------------------------------------------



Latent Diffusion Models can be viewed as a specialized adaptation of Denoising Probabilistic Diffusion Models, where the diffusion process is applied not directly to the high-dimensional data (like images), but to a lower-dimensional latent representation of the data. This adaptation can be concisely explained in a few steps:

### 1. **Latent Space Representation:**
In Latent Diffusion Models, the first step is to encode the high-dimensional data (e.g., images) into a lower-dimensional latent space. This is typically achieved using an encoder network, similar to the one used in Variational Autoencoders (VAEs):

$$ z_0 = \text{Encoder}(x) $$

Here, $$ x $$ is the original high-dimensional data, and $$ z_0 $$ is its latent representation.

### 2. **Applying the Diffusion Process to Latent Space:**
Once the data is in latent space, the diffusion process is applied. This process is similar to the one used in standard Denoising Probabilistic Diffusion Models, but it operates on the latent representations $$ z $$ rather than the original data $$ x $$:

- **Forward Process (Adding Noise in Latent Space):** This involves gradually adding noise to the latent representation over several time steps, transforming $$ z_0 $$ into a noisy version $$ z_T $$ which resembles a sample from a Gaussian distribution.

- **Reverse Process (Denoising in Latent Space):** The model learns to reverse this diffusion process, starting from the noisy latent representation $$ z_T $$ and progressively denoising it to recover the original latent representation $$ z_0 $$.

### 3. **Decoding to Original Space:**
After the reverse diffusion process in the latent space, the final step involves transforming the denoised latent representation back into the high-dimensional space:

$$ \hat{x} = \text{Decoder}(z_0) $$

### 4. **Advantages:**
Latent Diffusion Models benefit from operating in a lower-dimensional space, which can lead to:

- **Reduced Computational Complexity:** Since the latent space is lower-dimensional compared to the original data space, the model can be more computationally efficient.
- **Potential for Improved Learning Dynamics:** The lower-dimensional space may have simpler statistical properties, potentially aiding the learning process.

### Conclusion:
Latent Diffusion Models are an innovative approach that combines the strengths of diffusion models with the efficiency of latent space representations, offering a powerful method for generative tasks in machine learning.