# Plant-Disease-Classification-using-CNN
A deep learning project for classifying corn leaf images into four disease categories using an AlexNet-based CNN architecture. The project compares a baseline implementation with a modified architecture incorporating Batch Normalization, Global Average Pooling, and a lower learning rate to improve training stability and classification performance.

## Project Workflow
Dataset → EDA → Data Split → Preprocessing → Baseline AlexNet → Model Modification → Training → Evaluation

## Dataset
The dataset contains images of corn leaves from four categories:
* Common Rust
* Blight
* Healthy
* Gray Leaf Spot

The dataset showed some class imbalance, with **Gray Leaf Spot** containing fewer images than the other categories.

## Exploratory Data Analysis
EDA was performed to examine:
* Class distribution
* Image dimensions
* Aspect ratios
* Sample images from each category

The analysis identified an imbalance between classes, particularly for Gray Leaf Spot.

## Data Preparation

The dataset was divided using stratified sampling:
* **70% Training**
* **15% Validation**
* **15% Testing**

Images were resized to **224 × 224** pixels and normalized using pixel rescaling.

## Baseline Model
A CNN architecture based on **AlexNet** was implemented from scratch. The baseline architecture consisted of:
* 5 convolutional layers
* ReLU activation
* Max pooling layers
* 2 fully connected layers with 4096 neurons
* Dropout with a rate of 0.5
* Softmax output layer

The model was trained for 10 epochs using the Adam optimizer with its default learning rate of **0.001**.

## Modified Model
The baseline architecture was modified to improve training stability and reduce unnecessary model complexity.
### 1. Batch Normalization
Batch Normalization was added after the convolutional layers to stabilize activation distributions during training.

### 2. Global Average Pooling
The original `Flatten` layer was replaced with **Global Average Pooling (GAP)**.  
This significantly reduces the number of parameters passed to the fully connected layers compared with flattening the entire feature map.

### 3. Learning Rate
The learning rate was reduced:
```text
0.001 → 0.0001
```
A smaller learning rate was used to make weight updates more gradual and improve training stability.

### 4. Fully Connected Layer
The modified architecture uses a smaller dense layer with **1024 neurons** before the final classification layer.

## Modified Architecture
```text
Input (224 × 224 × 3)
        ↓
Conv2D (96) + BatchNorm + ReLU
        ↓
MaxPooling
        ↓
Conv2D (256) + BatchNorm + ReLU
        ↓
MaxPooling
        ↓
Conv2D (384) + BatchNorm + ReLU
        ↓
Conv2D (384) + BatchNorm + ReLU
        ↓
Conv2D (256) + BatchNorm + ReLU
        ↓
MaxPooling
        ↓
Global Average Pooling
        ↓
Dense (1024) + ReLU + Dropout
        ↓
Softmax Output (4 Classes)
```

## Results
| Metric              | Baseline |   Modified |
| ------------------- | -------: | ---------: |
| Validation Accuracy |   67.60% | **88.99%** |
| Test Accuracy       |      57% |    **84%** |
| Macro F1-Score      |     0.38 |   **0.77** |

The modified model achieved a substantial improvement in both overall accuracy and macro F1-score.

### Per-Class Performance
| Class          | Precision | Recall | F1-Score |
| -------------- | --------: | -----: | -------: |
| Common Rust    |      0.75 |   0.89 |     0.81 |
| Blight         |      0.96 |   0.90 |     0.93 |
| Healthy        |      0.76 |   0.29 |     0.42 |
| Gray Leaf Spot |      0.84 |   1.00 |     0.91 |

The model performed well on most disease categories, while the **Healthy** class remained more challenging, particularly in terms of recall.

## Evaluation
Model performance was evaluated using:
* Accuracy
* Precision
* Recall
* F1-Score

The modified model showed a much more balanced classification performance compared with the baseline model, although further improvement would be needed for the Healthy class.

## Future Improvements
Possible improvements include:
* Class-specific image augmentation, particularly for the Healthy class
* Transfer learning using pretrained CNN architectures
* Learning rate scheduling
* Expanding the dataset to include additional plant diseases
