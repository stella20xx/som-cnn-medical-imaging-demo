# som-cnn-medical-imaging-demo
Label-free SOM→U-Net CT/PET-CT imaging demo and related visualizations
# Label-Free SOM→U-Net Medical Imaging Demo

This repository hosts slide demos for my research on **label-efficient, interpretable imaging AI**.

## 1. CT/PET-CT SOM–CNN Demo

File: `LabelFree_SOM_CNN_CT_Demo.pdf`

- Pixel-level **Self-Organizing Map (SOM)** on morphology features to obtain repeatable, interpretable tissue/lesion prototypes.
- **Label-free SOM→U-Net teacher–student pipeline**: SOM runs in seconds per 200×200 CT-style slice on CPU; a lightweight U-Net reaches near-teacher performance (IoU ≈ 0.98, Dice ≈ 0.99) after minutes of single-GPU training.
- Uses **ΔIoU → 0** as a simple **AI QA/QC** criterion to detect instability and potential failure modes.
- Designed as an imaging backbone that can be combined with **structured EHR, lab values, and -omics data** for multi-modality medical AI.

Code will be released after related manuscripts are accepted.  
For now, this repository provides visual demos to accompany applications to **multi-modality medical AI postdoctoral positions**.
