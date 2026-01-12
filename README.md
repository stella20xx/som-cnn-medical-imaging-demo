# Label-Free CT-Style Lesion / Defect Segmentation with SOM–CNN

Label-free, pixel/voxel-level segmentation of CT-style lesions and
defects using a physics-aware Self-Organizing Map (SOM) teacher and a
lightweight CNN/U-Net student.

This demo shows how a SOM can provide full-resolution “teacher” masks in
seconds on CPU, and how a small U-Net student can match these masks with
IoU ≈ 0.99 and Dice ≈ 0.98 **without any manual voxel labels**.

See the full slide deck in  
`[docs/medical_ct_som_cnn_demo.pdf](https://github.com/stella20xx/som-cnn-medical-imaging-demo/blob/main/docs/medical_ct_som_cnn_demo.pdf)`  


---

## Problem

- Medical and industrial CT segmentation usually relies on expert-drawn
  voxel labels, which are:
  - expensive,
  - slow to obtain,
  - difficult to standardize across annotators.
- Goal: **obtain 3D lesion/defect masks with zero manual voxel labels**,  
  while keeping the model interpretable and easy to reproduce on a laptop.

---

## Data

- 3D CT-style volumes (slices) with visible lesions or defects.
- For this demo:
  - only **raw images** are used;
  - no ground-truth voxel masks are assumed during training.
- Evaluation is done by comparing the CNN/U-Net student to the SOM
  teacher masks at slice level.

---

## Method (SOM teacher → CNN/U-Net student)

1. **Physics/appearance-aware features**

   For each pixel/voxel in a CT slice:

   - intensity (optionally log-transformed),
   - local contrast and edge sharpness,
   - neighborhood texture / thinness,
   - simple statistics over a small spatial window.

   These form a low-dimensional, physics/appearance-aware descriptor
   for each location.

2. **PCA + SOM clustering**

   - PCA reduces feature dimension while preserving lesion vs background
     variance.
   - A Self-Organizing Map (SOM) is trained on a subset of slices to
     cluster pixels into a small number of classes (e.g.:
     background, soft tissue, lesion/defect).
   - SOM centroids (weight vectors) define a **reusable codebook** for
     the given imaging physics.

3. **SOM “teacher” masks**

   - The trained SOM is applied to full-resolution slices (arbitrary
     size) in seconds on CPU.
   - Clusters are interpreted (via feature profiles和空间分布) and
     grouped into a binary **lesion/defect vs non-defect** mask.
   - This yields a **cost-free / zero-cost pixel-level teacher**.

4. **CNN/U-Net “student”**

   - A lightweight U-Net is trained on CT slices, using the SOM masks as
     supervision (teacher–student).
   - The student learns a smooth voxel-level segmentation that matches
     the SOM teacher, with:

     - IoU ≈ 0.99  
     - Dice ≈ 0.98

     on held-out slices, while requiring **zero manual voxel labels**.

Key properties:

- **Label-free:** no expert voxel annotations are needed at any stage.
- **Pixel/voxel-level:** both SOM and U-Net operate on full slices,
  not on cropped patches.
- **Transferable:** SOM centroids/codebooks learned on some slices can
  be reused on other slices / volumes with the same imaging physics.
- **Reproducible on a laptop:** SOM teacher runs in seconds on CPU,
  U-Net student trains in minutes on a single-GPU laptop.

---

## Results (see PDF)

Highlights from `docs/medical_ct_som_cnn_demo.pdf`:

- SOM clustering produces clean lesion/defect masks with clear
  separation from background.
- U-Net students trained purely on SOM labels achieve IoU ≈ 0.99 and
  Dice ≈ 0.98 versus the SOM teacher on held-out slices.
- The pipeline scales to whole volumes (all slices), producing
  full-volume masks without manual voxel labeling.

---

## Status

This repository currently serves as a **documentation/demo hub** for the
CT-style SOM–CNN pipeline. Code and runnable notebooks will be added
progressively as the framework is cleaned and released.
