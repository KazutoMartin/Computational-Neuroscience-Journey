2026-06-03 01:13

Tags: [[Neuroscience]] [[Hemodynamics]] [[fMRI]] [[Signal]] [[Linear Algebra]] #Requirements


# fMRI Signal Processing

# What does it do?

fMRI measures brain activity by detecting changes associated with blood flow. It has high spatial resolution (millimeters) but low temporal resolution (seconds).


### Core Concepts
* [ ]   **4D Data Structures**: fMRI data is 4-dimensional: 3 spatial dimensions (voxels) over 1 time dimension (Volumes/TRs).
* [ ]   **Repetition Time (TR)**: The time between successive brain volumes.
* [ ]   **The HRF**: The mathematical model of how blood flow responds to a neural event (delayed by about 4-6 seconds).

### Preprocessing Pipelines
fMRI preprocessing is complex and usually requires a standardized pipeline (like *fMRIPrep*). You must understand the steps:
* [ ]   **Slice-Time Correction**: Interpolating data so all slices in a 3D volume appear to have been acquired at the exact same time.
* [ ]   **Realignment (Motion Correction)**: Re-aligning all volumes to a reference volume to correct for head movement using 6 rigid-body transformations (translations and rotations).
* [ ]   **Co-registration**: Aligning the low-resolution functional image (fMRI) to the high-resolution structural image (T1-weighted MRI).
* [ ]   **Spatial Normalization**: Warping the subject's brain to a standard brain template (e.g., MNI152) so multiple subjects can be compared.
* [ ]   **Spatial Smoothing**: Applying a Gaussian kernel to blur the images slightly, which increases the Signal-to-Noise Ratio (SNR) and helps satisfy statistical assumptions.

### Analysis Techniques
* [ ]   **The General Linear Model (GLM)**: The core statistical tool for task-based fMRI. You model the data as $Y = X\beta + \epsilon$, where $X$ is your design matrix (stimulus timings convolved with the HRF).
* [ ]   **First-Level vs. Second-Level Analysis**: First-level is within a single subject; second-level is a group analysis across multiple subjects.
* [ ]   **Resting-State Functional Connectivity**: Correlating the spontaneous BOLD time series between different brain regions (Seed-based correlation or Spatial ICA) to find resting-state networks (e.g., Default Mode Network).
* [ ]   **ROI Extraction**: Extracting signal averages from specific Regions of Interest based on brain atlases.


### Essential Software/Libraries
*   **Standalone Software**: `FSL`, `SPM` (MATLAB), `AFNI`.
*   **Python**: `Nilearn`, `NiBabel` (for reading NIfTI files), `fMRIPrep` (for automated preprocessing).


# References

[[EEG and fMRI signals processing requirements]]