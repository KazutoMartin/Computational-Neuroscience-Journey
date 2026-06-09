2026-06-03 01:08

Tags: [[Neuroscience]] [[EEG]] [[Signal]] [[Neurophysiology]] #Requirements


# EEG Signal Processing

## What does it do?
EEG measures the electrical activity of the brain via electrodes on the scalp. It has high temporal resolution (milliseconds) but low spatial resolution.

## Core Concepts
* [ ]   **Time-Series Data**: Understanding sampling rates (e.g., $250 \text{ Hz}$ to $1000 \text{ Hz}$), microvolt ($\mu V$) amplitude scales, and the 10-20 international electrode placement system.
* [ ]   **Frequency Bands**: Delta ($<4 \text{ Hz}$), Theta ($4-8 \text{ Hz}$), Alpha ($8-13 \text{ Hz}$), Beta ($13-30 \text{ Hz}$), and Gamma ($>30 \text{ Hz}$).

## Preprocessing Skills
* [ ]   **Filtering**: Designing and applying band-pass filters (to isolate brain waves) and notch filters (to remove $50/60 \text{ Hz}$ power line noise).
* [ ]   **Artifact Rejection**: Identifying and removing eye blinks, muscle activity (EMG), and heartbeats (ECG).
* [ ]   **Independent Component Analysis (ICA)**: A mathematical technique used to separate the EEG signal into independent sources to isolate and subtract artifacts.
* [ ]   **Referencing**: Re-referencing data to an average reference, mastoids, or a single electrode.
* [ ]   **Epoching**: Slicing continuous EEG data into time windows (epochs) locked to specific events or stimuli.


## Analysis Techniques
* [ ]   **Time Domain - ERPs (Event-Related Potentials)**: Averaging epochs to find event-locked signals (e.g., P300, N400).
* [ ]   **Frequency Domain**: Using FFT or Welch's method to calculate the Power Spectral Density (PSD).
* [ ]   **Time-Frequency Analysis**: Using Morlet Wavelets or the Hilbert Transform to see how frequency power changes over time (ERSP - Event-Related Spectral Perturbation).
* [ ]   **Functional Connectivity**: Calculating phase synchronization, Coherence, or Phase-Locking Value (PLV) between different electrodes.


## Essential Software/Libraries
*   **Python**: `MNE-Python` (the absolute standard).
*   **MATLAB**: `EEGLAB`, `FieldTrip`, `Brainstorm`.
# References


[[EEG and fMRI signals processing requirements]]