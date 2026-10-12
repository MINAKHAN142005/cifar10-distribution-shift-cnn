
# cifar10-distribution-shift-cnn
Small CNN trained on 400 CIFAR-10 images, studying how photometric augmentation and regularisation improve robustness to a distribution-shifted test set.
# CIFAR-10 Distribution Shift CNN

A small convolutional neural network trained on only **400 CIFAR-10 images** and evaluated on a distribution-shifted test set. The project studies which regularisation and data augmentation choices help a low-data model stay accurate when test images differ from the training images.

Built for *Deep Learning for Perception*, Assignment 2, Section B, Question 1.

## Results

Mean ± standard deviation over training seeds 0, 1 and 2.

| Metric | Result |
|---|---|
| Validation accuracy | 57.00 ± 3.00% |
| Clean test accuracy | 44.72 ± 1.26% |
| Shifted test accuracy | 18.55 ± 0.31% |
| Shifted test accuracy with TTA | 19.39 ± 0.06% |
| Baseline shifted accuracy | 16.33 ± 1.10% |

The final model improves shifted test accuracy by **2.22 percentage points** over the baseline.

## Final Configuration

This configuration had the highest mean validation accuracy. It was selected without using test results.

| Component | Setting |
|---|---|
| Architecture | Small CNN, 196,634 parameters (Conv, BatchNorm, ReLU, MaxPool blocks, one Inception module, global average pooling) |
| Regularisation | Dropout 0.5, weight decay 0.0005 (AdamW), best-validation checkpoint |
| Training | 400 images, 60 epochs, batch size 64, learning rate 0.001, cosine schedule |
| Augmentation | Photometric only: colour jitter 0.4, Gaussian blur, Gaussian noise |

## Key Findings

- **Photometric augmentation helped most.** Against the same regularised model without augmentation, it raised shifted test accuracy from 16.15% to 18.55% and clean test accuracy from 41.73% to 44.72%. It matches the brightness, contrast, blur and noise changes in the shifted test set.
- **Geometric augmentation** improved clean accuracy but not shifted accuracy.
- **Random Erasing and CutMix** reduced validation accuracy by about 11 points, as they are too strong for a model trained on 400 images.
- **The channel swap is the hardest shift.** On its own it drops clean accuracy from 44.72% to 29.41%, because colour jitter never swaps channels during training.

## The Distribution Shift

The shifted test set combines several corruptions at once:

- Rotation
- Colour changes
- Blur
- Noise
- Channel swap
- 25% occlusion

## Getting Started

```bash
git clone https://github.com/usman-dilbar/cifar10-distribution-shift-cnn.git
cd cifar10-distribution-shift-cnn
pip install -r requirements.txt
```

Then open the notebook or run the training script (replace with your actual file name):

```bash
python <your_script>.py --seed 0
```

CIFAR-10 is downloaded automatically on the first run.

## Author

Roll No. 23F-0618, BSCS, Section 7A
