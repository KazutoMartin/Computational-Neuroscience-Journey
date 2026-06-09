2026-06-03 01:20

Tags: [[Neuroscience]] [[EEG]] [[fMRI]] [[Linear Algebra]] [[Machine Learning]] [[Probability and Statistics]] #Requirements


# Neuroscience Machine Learning and Advanced Analytics

Once you can process the raw signals, modern neuroscience often applies Machine Learning for decoding and Brain-Computer Interfaces (BCI).


- [ ] **Feature Extraction**: Turning EEG matrices or fMRI maps into flat feature vectors.
* [ ]   **Dimensionality Reduction**: PCA (Principal Component Analysis) and t-SNE / UMAP for visualizing high-dimensional brain states.
* [ ]   **Traditional ML**: Support Vector Machines (SVM), Random Forests, and Logistic Regression (often implemented using `scikit-learn`).
* [ ]   **Deep Learning**: 
    * [ ]   *EEG*: Convolutional Neural Networks (CNNs) like EEGNet, or Recurrent Neural Networks (LSTMs) for time-series.
    * [ ]   *fMRI*: 3D CNNs or Graph Neural Networks (GNNs) applied to brain connectivity matrices.
* [ ]   **Cross-Validation**: Strict separation of training and testing data, using techniques like Leave-One-Subject-Out (LOSO) cross-validation to prevent data leakage.



# References

[[EEG and fMRI signals processing requirements]]