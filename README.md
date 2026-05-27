## Overview
The pipeline consists of four main stages:
1. MEG data simulation
2. Preprocessing
3. Time-Delay Embedding & HMM training
4. Ground-truth evaluation

This repository contains two main notebooks. Each having different purpose:

1) `main.ipynb`
This notebook provides a complete pipeline for training and evaluating a 3-state TDE-HMM on simulated MEG data
This default configuration includes state:
  - Baseline
  - Alpha
  - Gamma
This notebook serve as a default reference implementation of the TDE-HMM piepline. Works best as an example to study how the pipeline works out.

2) `6_state_main.ipynb`
This notebook extends the pipeline to support additional oscillatory states (e.g., Delta, Theta, Beta).
This notebook includes detailed inline comments that explains:
  - Which lines of code correspond to individual states
  - Where to modify the code to add or remove states
This notebook serve as a guide on how to modify code to generate visuals (histogram, confusion matrix heatmap) with more than 3 states. Please modify the code accordingly to generate plot for 4 states, 5 states, etc.

This pipeline was developed and ran using Python 3.9, and is recommended for reproducing the results as well to avoid any dependency errors. It is also highly recommended to run this notebook locally rather than via cloud (such as Google Colab), unless there is access to Colab's premium GPUs. Data simulation is reported to take much longer via cloud than locally. 

All necessary components to run the notebook are included in the notebook. Each cell should be run in the order that they appear in to ensure correct functionality. 

## Dependencies
Install all required packages before running, included in the `main.ipynb` file:
```
pip install numpy scipy matplotlib mne nibabel glhmm scikit-learn
```
## Generating Simulated Datasets
To generate multiple simulations:
```
generate_multiple_datasets(
    fwd=fwd,
    src=src,
    info=info,
    n_simulations=4,
    save_dir="simulations"
)
```
Each dataset is stored under:
```
simulations/
  sim_000/
    raw.fif
    state_labels.npy
  sim_001/
  ...
```

## HMM Training
A 3-state TDE-HMM is trained following from the tutorial from glhmm [here]([url](https://github.com/vidaurre/glhmm/blob/57c2f7995eb5087f6e2e132c47a7dd692a55363d/docs/notebooks/HMM-TDE_vs_HMM-MAR_example.ipynb)).
The trained model is saved as `hmm_tde.pkl`

## Evaluation
Because HMM staet labels are permutation-invariant, predicted states are matched to true states using the Hungarian algorithm before evaluation.

## Visualization
The notebook includes Raw MEG signal inspection, power spectral density plots, state-over-signal plots, and state occupancy and lifetime statistics. These serve as sanity checks rather than formal analyses. 

In addition, there are histogram that compare distribution of states in TDE-HMM VS Ground-Truth and Confusion Matrix heatmap at the end of the notebook. These graphs help visualize the effectiveness of trained TDE-HMM.

### Notes and Limitations
Transition probabilities are fixed and not varied across simulations. Only two oscillatory states (alpha, gamma) are modeled. Noise structure is also simplified.
