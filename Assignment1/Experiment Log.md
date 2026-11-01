## Notes

### Original baseline

**Changes:** Supplied model and settings, with no augmentation or added regularisation. Used all 50,000 training images and monitored the test set.

**Findings:** The widening gap between training and test performance showed overfitting.

**Result:**
- Final training accuracy: 98.2%
- Final test accuracy: 76.5%
- Best test accuracy: 77.5% at epoch 10

### Validation baseline

**Changes:** Set aside 5,000 training images for validation; otherwise kept the baseline settings.

**Findings:** Validation loss was lowest at epoch 10, then rose as training continued. The model was overfitting. This validation result and the original test result use different splits.

**Result:**
- Final training accuracy: 97.9%
- Final and best validation accuracy: 78.8%

### Experiment 1: crop and flip

**Changes:** Added random crops and horizontal flips to training images.

**Findings:** The extra variation likely helped the model generalise. Training accuracy was measured on augmented images.

**Result:**
- Final training accuracy: 84.1%
- Final validation accuracy: 83.2%
- Final validation loss: 0.4880

### Experiment 2: 40 epochs

**Changes:** Kept crop and flip and trained for 40 epochs instead of 20.

**Findings:** The model benefited from more training, although validation loss rose again after epoch 32.

**Result:**
- Best validation accuracy: 87.3% at epoch 32
- Final validation accuracy: 87.0%
- Final training accuracy: 93.3%
- Lowest validation loss: 0.3747 at epoch 32

### Experiment 3: learning rate schedule

**Changes:** Kept the 40 epoch setup and reduced the learning rate from 0.01 to 0.001 after epoch 30. The best validation checkpoint is restored for evaluation.

**Findings:** Pending.

**Result:** Pending.
