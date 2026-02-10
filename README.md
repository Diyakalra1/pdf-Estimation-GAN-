# PDF-Estimation-Using-GAN

## Overview

This assignment focuses on learning an unknown probability density function (PDF) directly from data samples, without assuming any analytical or parametric form. A Generative Adversarial Network (GAN) is used to implicitly model the distribution of a transformed random variable derived from real-world Indian air quality data.

The dataset consists of NO₂ concentration values, which are first transformed using a nonlinear function. The transformed data is then used to train a GAN to learn the underlying probability distribution.

---

## Dataset Description

**Dataset Used:** Indian Air Quality Dataset  
**Source:** https://www.kaggle.com/datasets/shrutibhargava94/india-air-quality-data  

The NO₂ concentration feature is selected because it is continuous, real-valued, and exhibits a non-Gaussian distribution, making it suitable for density estimation using generative models. The dataset contains 16,233 missing values, which are removed before further processing.

---

## Methodology

### Step 1: Data Transformation

Each NO₂ concentration value `x` is transformed into a new variable `z` using the nonlinear transformation:
z = x + ar · sin(br · x)


Where:
- `ar = 0.5 × (r % 7)` → Calculated value: **2.0**
- `br = 0.3 × (r % 5 + 1)` → Calculated value: **0.3**
- `r` is the university roll number

---

### Step 2: GAN-Based PDF Learning

#### GAN Architecture

The Generative Adversarial Network (GAN) consists of two neural networks: a Generator and a Discriminator.

- **Generator:**  
  Takes random noise sampled from a normal distribution and maps it to synthetic samples of the transformed variable `z`. It is implemented as a fully connected neural network with ReLU activations and outputs a single scalar value. Its objective is to generate samples that resemble real data.

- **Discriminator:**  
  A fully connected neural network that takes a single input value and outputs a probability indicating whether the sample is real or generated. A sigmoid activation function is used in the final layer for binary classification.

Both networks are trained simultaneously in an adversarial manner. The discriminator learns to distinguish real samples from fake ones, while the generator learns to fool the discriminator. Through this process, the generator implicitly learns the probability distribution of `z` without assuming any parametric form.

---

## Training the GAN

- **Loss Function:** Binary Cross-Entropy Loss  
- **Optimizer:** Adam  

### Training Strategy
- The discriminator is trained using both real samples and generated samples.
- The generator is trained to maximize the discriminator’s confidence that generated samples are real.

Training continues until both networks reach a stable equilibrium.

---

## PDF Estimation from Generated Samples

After training:
- A large number of samples are generated using the trained generator.
- The probability density function is estimated using:
  - Histogram-based density estimation

This estimated PDF represents the learned distribution of the transformed variable `z`.

---

## Results

A histogram comparing the real PDF and the GAN-generated PDF demonstrates that the generated distribution closely matches the real data distribution.

---

## Observations

### 1. Mode Coverage
The generator successfully captures the main regions where the real data is concentrated. The generated samples span the same major value ranges as the real samples, indicating good mode coverage.

### 2. Training Stability

**Discriminator Loss (D Loss):**
- Remains approximately between 1.3 and 1.5
- Does not collapse to zero
- Does not diverge to large values  

This indicates that the discriminator is neither overpowering nor failing.

**Generator Loss (G Loss):**
- Fluctuates between approximately 0.6 and 1.0
- Expected oscillations are observed during adversarial training
- No continuous increase or collapse occurs  

The bounded and smooth oscillations of both losses indicate stable and balanced GAN training.

### 3. Quality of Generated Distribution

The quality of the generated distribution is evaluated by comparing the histogram of GAN-generated samples with the real data distribution. Both distributions exhibit a similar shape and are concentrated in the same value range. Although minor differences in peak height and tail behavior exist, the generator successfully captures the main structure of the data and effectively learns the underlying probability distribution.
