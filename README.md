# Full-FingerPrint-Reconsturction
Reconstructing a complete fingerprint from a partial image

This project implements a Generative Adversarial Network (GAN) for reconstructing complete fingerprint images from partial fingerprints.

The model consists of a Generator that learns to recover missing fingerprint information and a Discriminator that distinguishes generated fingerprints from real fingerprint images. The notebook includes dataset preprocessing, GAN training, fingerprint generation, model saving/loading, visualization, and performance evaluation using Structural Similarity Index (SSIM) and discriminator accuracy.

The project is implemented using Python and PyTorch and supports GPU acceleration when CUDA is available.

DataSet : https://www.kaggle.com/datasets/ruizgara/socofing/data
