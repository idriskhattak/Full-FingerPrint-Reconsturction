# Full Fingerprint Reconstruction

A Generative Adversarial Network that reconstructs complete fingerprint images from
partial ones, trained on SOCOFing and evaluated with SSIM and discriminator accuracy.

📄 Full write-up: [idriskhattak.github.io/Idris_Portfolio/projects/fingerprint-reconstruction/](https://idriskhattak.github.io/Idris_Portfolio/projects/fingerprint-reconstruction/)

## The problem

A fingerprint capture can be incomplete because of the angle, the sensor coverage, or
how the finger was placed. The goal is to recover a plausible full image from a
partial one while keeping the result close enough to the original to be useful.

## How it works

The **generator** takes a partial fingerprint and learns to fill in the missing
structure. The **discriminator** sees real and generated images and learns to tell
them apart. Their competing objectives give the generator a training signal for
producing more realistic reconstructions.

The notebook covers preprocessing, GAN training, generated-image inspection, model
saving and loading, and evaluation. It uses CUDA when available.

## Results

Two evaluation runs, both reported rather than only the better one:

| | First run | Later run |
| --- | --- | --- |
| Average SSIM | 0.6421 | 0.6127 |
| Discriminator accuracy, real images | 99.28% | 99.88% |
| Discriminator accuracy, generated images | 91.57% | 91.98% |

An SSIM around 0.62–0.64 is moderate structural similarity — the reconstruction
resembles the original, it is not a recovery of it. The discriminator is near-perfect
on real images and still identifies roughly 92% of generated ones as fake, which is
the useful diagnostic here: visual plausibility and reconstruction fidelity are
related but they are not the same result.

The gap between the two runs also matters. One checkpoint or one good-looking sample
is not enough to establish reconstruction quality.

## Running it

```bash
pip install torch torchvision scikit-image opencv-python matplotlib
jupyter notebook fingerprint.ipynb
```

**Dataset:** [SOCOFing (Sokoto Coventry Fingerprint Dataset)](https://www.kaggle.com/datasets/ruizgara/socofing/data) — download it
from Kaggle and point the notebook's data path at the extracted folder.

## Known limitations

- **No held-out test protocol.** SSIM is computed over generated samples, not a fixed
  evaluation split defined before training.
- **No baseline comparison.** The GAN is not measured against a simpler
  image-completion method, so the generative approach is unjustified in numbers.
- **Checkpoints were saved and resumed at different points**, which makes the two runs
  less directly comparable than they should be.
- **Masking is not held constant** between experiments.

## What I would do differently

Define the partial-image mask and the evaluation split before training. Compare
against a non-generative baseline. Report SSIM across the full held-out set, add a
perceptual metric, and check whether the reconstructed ridge structure is still
useful for downstream fingerprint matching — which is the question that would
actually decide whether this is worth anything.
