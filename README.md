# Quant-Coherency-dominant_direction
Scripts used in Actin Cable Organizing Center paper to quantify the coherency and dominant angle of the actin cables

# FIJI Macros for Actin Cable Orientation Quantification

## Overview
This repository contains a set of **FIJI macros** for automated quantification of actin cable orientation in budding yeast.  
The workflow integrates **OrientationJ** for orientation analysis and **Cellpose** for cell segmentation.  
While designed for super-resolution images, widefield data can also be used after **deconvolution** and **background subtraction** to improve signal quality.

---

## Prerequisites

### Required Software
- [FIJI (ImageJ)](https://fiji.sc/)
- [OrientationJ plugin](https://bigwww.epfl.ch/demo/orientation/)
- [Bio-Formats plugin](https://www.openmicroscopy.org/bio-formats/)
- [Cellpose](https://www.cellpose.org/)

### Recommended
- Super-resolution images  
- Deconvolved widefield images with background removal before MIP generation

### Supported File Types
`.tif`, `.czi`, `.nd2`, `.ome.tiff`  
Other formats supported by Bio-Formats can be added by modifying the macro header.

---

## Step 1 – Generate Maximum Intensity Projections and Masks

**Macro:** `AOC-maxproj_in_mask_folder-select_ch-v2.ijm`

1. Launch the macro in FIJI.  
2. Select the **channel** for MIP generation.  
3. Choose the **input folder** containing your raw image files.  
4. The macro creates a subfolder named `x-Mask` within the input folder.  
5. MIP images will be saved in the `x-Mask` folder for Cellpose segmentation.

---

## Step 2 – Cell Segmentation Using Cellpose

1. Open **Cellpose** and load MIP images from the `x-Mask` folder.  
2. Adjust **cell diameter** to match yeast cell size for optimal segmentation.  
3. Manually refine masks to ensure **mother and bud** are separated.  
4. Save each corrected mask as a `.png` file (`Ctrl + N`) in the same `x-Mask` folder.

---

## Step 3 – Quantify Actin Cable Orientation

**Macro:** `Quant-Coherency-dominant_direction.ijm`

1. Run the macro and specify:
   - **Input folder:** where MIP and mask files are located  
   - **Output folder:** where results will be saved  
2. Define:
   - **Actin channel** (required)  
   - **Mitochondrial channel** (optional)  
   - **Crop size** based on cell dimensions  
     - If too small, the macro may crop part of the cell.  
3. **Patch removal:**  
   - Bright actin patches can bias orientation results.  
   - When prompted, set a **threshold** to remove bright patches while preserving cables.  
   - If your sample lacks bright structures, you may skip this step.  
4. Enable **Batch Mode** to hide windows during processing.

---

## Step 4 – Define ROIs and Cell Axis

1. **Select cells** for analysis by clicking near the center of each mother cell (preferably near the bud neck).  
   - The macro saves these as **Cell number ROIs**.  
2. Define the **mother–bud axis**:
   - Draw a line from the **mother distal tip** perpendicular to the **bud neck**.  
   - The macro calculates the mother–bud axis angle and rotates each cell accordingly.

---

## Step 5 – Orientation Analysis

1. For each selected cell, the macro:
   - Crops the image based on your defined crop size.  
   - Prompts you to use the **wand tool** to select the **mother cell** from the Cellpose mask.  
   - Extracts the actin structure, aligns it to the mother–bud axis, and runs **OrientationJ** analysis.  
2. The macro generates:
   - **Coherency** and **dominant angle** measurements in the `1-Coherency` folder.  
   - **Intermediate files** for inspection and reproducibility:
     - Patch masks  
     - Threshold values  
     - Cell-numbered image  
     - Cell number ROIs  
     - Mother–bud axis ROIs  
     - Mother cell mask ROIs  
     - Cleaned and rotated cell images for OrientationJ

---

## Output Summary

| Output Type | Description | Folder |
|--------------|-------------|--------|
| Coherency and dominant angle | OrientationJ measurements for each cell | `1-Coherency` |
| Patch masks | Regions of bright actin patches removed prior to analysis | output folder |
| Cell number ROIs | Cell identifiers saved during manual selection | output folder |
| Axis ROIs | Mother–bud axis lines | output folder |
| Rotated cell images | Aligned actin structures used for orientation analysis | output folder |

---

## Notes
- Ensure **mother and bud** are properly separated in the Cellpose mask before running the quantification macro.  
- Adjust **crop size** and rotation parameters in the macro header as needed.  
- For **cell morphology quantification**, refer to the dedicated morphology macro (provided separately).  

---

## Citation
If you use these macros in your research, please cite:  
> Yang, E.J.-N., Filpo, K., Boldogh, I., Swayne, T.C., and Pon, L.A. "Tying up loose ends: an actin cable organizing center contributes to actin cable polarity, function and quality control in budding yeast." submitted. [2025]


---

## License
This project is released under the [MIT License](LICENSE).

---

## Contact
For questions, issues, or suggestions, please contact:  
**[Emily J. Yang]**  
Email: [emily.jiening.yang@gmail.com]  
