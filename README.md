# central-nuclei-muscle-analyzer
A comprehensive ImageJ macro for automated analysis of nuclear localization in muscle cross-sectional areas, specifically designed to identify and quantify central nuclei in muscle fibers.

## Overview
This tool performs automated detection and classification of muscle fibers based on nuclear positioning, distinguishing between normal (peripheral nuclei) and abnormal (central nuclei) muscle fibers. It's particularly useful for studying muscle pathology and regeneration.

![workflow](/workflow_overview/workflow_overview.jpg)

## Features

- **Automated muscle fiber segmentation** using Cellpose-generated masks
- **Nuclear detection** using color deconvolution and Difference of Gaussian filtering
- **Central nuclei identification** with customizable edge-to-center distance
- **Batch processing** for multiple images
- **Comprehensive output** including ROI files, classified images, and summary statistics
- **GPU acceleration** using CLIJ2 for improved performance

## Prerequisites

### Required Software and Plugins
- **Cellpose** for muscle-crossal section segmentation
- **Fiji** 
- **CLIJ/CLIJ2** plugin for ImageJ

## Input Requirements

### Image Preparation
1. **Original images**: H&E stained muscle cross-sections (`.tif` or `.jpg` format)
2. **Mask generation**: Use Cellpose to generate segmentation masks


### Cellpose Batch Command Example (Process Entire Folder)

```bash
python -m cellpose --dir . --pretrained_model cyto3 --diameter 100. --verbose --save_png --no_npy
```

**Notes**:  
- The `cyto3` or `cyto2` models generally perform well.  
- The `CPSAM` model is **not recommended** for the sample images provided.  

**Cellpose Installation**:  
- Official repository: [MouseLand/cellpose](https://github.com/MouseLand/cellpose)  
- To install a specific (older) version:  
  ```bash
  pip install "cellpose[gui]==3.1.1.2"
  ```

## Usage
![GUI](/workflow_overview/GUI.png)


### Parameters
- **Format**: Choose between `.tif` or `.jpg` for input images
- **Input Folder**: Directory containing original images and Cellpose masks
- **Output Folder**: Directory for results
- **Pixel Scale X/Y**: Calibration values for spatial measurements
- **CSA Size Range**: Minimum and maximum cross-sectional area in pixels
- **Nuclei-to-CSA edge distance threshold**: The minimum distance from nuclei to the CSA edge, used as the erosion radius to define the central region for abnormal nuclei detection.
- **Line Width**: Width for visualization overlays

### Running the Analysis
1. Open ImageJ/Fiji
2. Load the macro file (`central-nuclei-muscle-analyzer_v1.ijm`)
3. Run the macro and set parameters in the dialog
4. Select input and output directories
5. The macro will process all images automatically

## Output Files

For each processed image, the following files are generated:

### ROI Files
- `*_ROI_CSA.zip`: Complete muscle fiber regions
- `*_ROI_Center.zip`: Central regions of muscle fibers
- `*_ROI_Classify.zip`: Classified ROIs (Green: Normal / Red: Abnormal)

### Images
- `*_Nuclei.tif`: Nuclear detection mask
- `*_Classified.tif`: Visualization with color-coded classification
  - **Green**: Normal fibers (peripheral nuclei)
  - **Red**: Abnormal fibers (central nuclei)

### Data Files
- `*_Summary.csv`: Detailed measurements for each fiber
- `Result_Summary.csv`: Overall summary statistics

## Workflow

1. **Preprocessing**: Load Cellpose masks and apply size/edge filters
2. **Central Region Definition**: Erode fiber boundaries to define central areas
3. **Nuclear Detection**: 
   - Color deconvolution (H&E separation)
   - Difference of Gaussian filtering
   - Thresholding and mask creation
4. **Classification**: Determine if nuclei are present in central regions
5. **Visualization**: Generate color-coded output images
6. **Export**: Save ROIs, measurements, and summary statistics

## Technical Details

### Image Processing Pipeline
- **Color Deconvolution**: Separates H&E staining components
- **Difference of Gaussian**: Enhances nuclear structures (σ=2 and σ=6)
- **Morphological Operations**: Erosion to define central regions
- **GPU Processing**: CLIJ2 for efficient label operations

### Classification Criteria
- **Normal Fiber**: No nuclei detected in central region
- **Abnormal Fiber**: One or more nuclei detected in central region


## Reference Citation

Please cite the following when using this tool:
1. **FIJI**:
   - Schindelin, J., Arganda-Carreras, I., Frise, E., Kaynig, V., Longair, M., Pietzsch, T., ... & Cardona, A. (2012). Fiji: an open-source platform for biological-image analysis. *Nature Methods, 9*(7), 676-682. [doi:10.1038/nmeth.2019](https://doi.org/10.1038/nmeth.2019)
2. **CLIJ2**:
   - Haase, R., Royer, L. A., Steinbach, P., Schmidt, D., Dibrov, A., Schmidt, U., ... & Myers, E. W. (2020). CLIJ: GPU-accelerated image processing for everyone. *Nature Methods, 17*, 5-6. [doi:10.1038/s41592-019-0650-1](https://doi.org/10.1038/s41592-019-0650-1)
   - Vorkel, D., & Haase, R. GPU-accelerating ImageJ Macro image processing workflows using CLIJ. [*arXiv preprint*.](https://arxiv.org/abs/2008.11799)
   - Haase, R., Jain, A., Rigaud, S., Vorkel, D., Rajasekhar, P., Suckert, T., ... & Myers, E. W. Interactive design of GPU-accelerated Image Data Flow Graphs and cross-platform deployment using multi-lingual code generation. [*bioRxiv preprint*.](https://www.biorxiv.org/content/10.1101/2020.11.19.386565v1)
3. **Cellpose**:  
   If you use Cellpose-SAM, please cite the Cellpose-SAM paper:  
   - Pachitariu, M., Rariden, M., & Stringer, C. (2025). Cellpose-SAM: superhuman generalization for cellular segmentation. bioRxiv.

   If you use Cellpose 1, 2 or 3, please cite the Cellpose 1.0 paper:  
   - Stringer, C., Wang, T., Michaelos, M., & Pachitariu, M. (2021). Cellpose: a generalist algorithm for cellular segmentation. Nature methods, 18(1), 100-106.

   If you use the human-in-the-loop training, please also cite the Cellpose 2.0 paper:  
   - Pachitariu, M. & Stringer, C. (2022). Cellpose 2.0: how to train your own model. Nature methods, 1-8.

   If you use the new image restoration models or cyto3, please also cite the Cellpose3 paper:  
   - Stringer, C. & Pachitariu, M. (2025). Cellpose3: one-click image restoration for improved segmentation. Nature Methods.
