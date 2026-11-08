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

**Findings:** Validation accuracy improved after the learning rate dropped, although the different random split and training randomness mean the schedule was not the only difference from the previous run.

**Result:**
- Best validation accuracy: 88.4% at epoch 34
- Final validation accuracy: 88.1%
- Final training accuracy: 94.9%
- Lowest validation loss: 0.3915 at epoch 31

## Experiment results

These runs used the same randomly generated training and validation split, 40 epochs, crop and flip, and a learning rate reduction after epoch 30. Training was unseeded. Results are ordered by each run's best validation accuracy.

| Experiment | Best validation accuracy | Epoch |
| --- | ---: | ---: |
| Reflection + batch normalisation + dropout 0.5 + weight decay | 91.96% | 40 |
| Reflection + batch normalisation + dropout 0.3 + weight decay | 91.84% | 39 |
| Reflection + batch normalisation | 91.76% | 38 |
| Reflection + batch normalisation + dropout 0.3 | 91.56% | 39 |
| Reflection crop and flip | 88.70% | 37 |
| Reflection + dropout 0.3 | 88.64% | 32 |
| Crop and flip control | 88.56% | 35 |

Earlier runs used a different split and are recorded in Notes.

### Test evaluations

Models were selected by validation accuracy. Training accuracy below was measured separately on the unaugmented training subset in evaluation mode.

| Model | Training accuracy | Test accuracy | Gap |
| --- | ---: | ---: | ---: |
| Reflection + batch normalisation | 98.93% | 90.74% | 8.19 points |
| Reflection + batch normalisation + dropout 0.5 + weight decay | 98.05% | 91.29% | 6.76 points |

The second model exceeds 90% test accuracy, but its gap is above the rubric's 5 point threshold. The test set has been inspected twice; further selection using its results would weaken its role as an independent evaluation. These checkpoints were trained by the local experiment runner and are saved in the ignored experiments folder.

## Further validation comparisons

These runs use the saved common random split. The gap compares unaugmented training accuracy in evaluation mode with validation accuracy at the best validation checkpoint. Test data was not used for these comparisons.

| Configuration | Validation accuracy | Unaugmented training accuracy | Gap | Best epoch |
| --- | ---: | ---: | ---: | ---: |
| onecycle | 92.24% | 99.21% | 6.97 points | 39 |
| weight_decay | 92.16% | 97.86% | 5.70 points | 37 |
| current | 91.96% | 98.05% | 6.09 points | 40 |
| conv_dropout | 91.10% | 96.11% | 5.01 points | 38 |

Selected: conv_dropout. No run met the under 5 point validation gap; this run had the smallest gap among those above 90% validation accuracy.

## Additional regularisation comparisons

All comparisons use the same saved split and evaluate training without augmentation at the best validation checkpoint. Test data was not used.

| Configuration | Epoch budget | Best epoch | Validation | Training | Gap |
| --- | ---: | ---: | ---: | ---: | ---: |
| dropout015_wd | 60 | 60 | 91.42% | 97.30% | 5.88 points |
| dropout015 | 60 | 54 | 91.26% | 96.32% | 5.06 points |
| random_erasing | 60 | 55 | 90.96% | 96.81% | 5.85 points |
| dropout015 | 40 | 40 | 90.74% | 95.18% | 4.44 points |
| dropout015_wd | 40 | 38 | 90.68% | 95.35% | 4.67 points |
| dropout020 | 60 | 51 | 90.62% | 95.12% | 4.50 points |
| dropout020 | 40 | 38 | 90.36% | 94.02% | 3.66 points |
| random_erasing | 40 | 39 | 90.22% | 95.57% | 5.35 points |

Dropout 0.15 at 40 epochs had the highest validation accuracy among runs with a gap under 5 points.
