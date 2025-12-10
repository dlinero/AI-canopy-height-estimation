# Using radar and deep learning to estimate canopy height across the tropics

![Python](https://img.shields.io/badge/Python-3.x-blue?style=flat&logo=python)
![Framework](https://img.shields.io/badge/Framework-PyTorch-orange?style=flat&logo=pytorch)
![Topic](https://img.shields.io/badge/AI4EO-Earth%20Observation-green)

## Motivation
Tropical and Subtropical Moist Broadleaf Forests (TSMBF) are home to most of the world's known wild species and play a vital role in regulating the climate by storing large amounts of carbon. Earth Observation (EO) imagery has been key for monitoring the extent and state of these forests across large areas and over long periods. While optical satellite imagery has traditionally been the main source of data for such analyses, Synthetic Aperture Radar (SAR) imagery is increasingly important for its ability to penetrate the vertical structure of forests and capture details that optical sensors often miss. SAR is also especially useful in tropical regions, because it can collect data regardless of persistent cloud cover. These features make SAR data particularly well-suited for studying changes in TSMBF, as it is susceptible to structural attributes such as canopy height—a key biodiversity variable for remote sensing that serves as both an indicator of habitat heterogeneity for wildlife and a proxy for forest biomass and carbon stocks.

This repository was developed as the final project for the **AI for Earth Observation** course of the University of Wisconsin-Madison, taught by [Dr. Jonathan A. Sullivan](https://jonathanasullivan.github.io/). 

## Research Questions

1.  How well do different deep learning architectures predict canopy height from radar and topographic data across the TSMBF?
2.  How do transfer learning and data augmentation affect model accuracy?
3.  Which architecture–training strategy combination achieves the lowest prediction loss?

## Instructions for running code

**Prerequisites:**
* You need a valid Google Earth Engine account and a Google Drive account with sufficient storage space.
* A GPU runtime is strongly recommended (the code has not been verified on CPU-only environments).
* All necessary Python libraries are listed in the notebooks and will install automatically.

**Workflow:**
1.  Upon running the cells, grant Google Colab permission to mount your Google Drive and authenticate with Earth Engine.
2.  **Step 1:** Run the **Downloading and structuring data** notebook first. This extracts the necessary satellite imagery and saves the dataset to your Google Drive.
3.  **Step 2:** Run the **Canopy height regression** notebook. This reads the saved imagery from your Drive to perform modeling. 

## Dataset description

The dataset was generated using the **Downloading and structuring data** notebook. It consists of 999 image tiles ($64 \times 64$ pixels) sampled randomly across the Tropical and Subtropical Moist Broadleaf Forests (TSMBF) biome, defined by the [RESOLVE Ecoregions 2017](https://developers.google.com/earth-engine/datasets/catalog/RESOLVE_ECOREGIONS_2017) map.

Each tile is composed of 5 channels, all harmonized to a 10-meter spatial resolution:

| Channel | Variable | Details & Source |
| :--- | :--- | :--- |
| **1** | Radar Backscatter (VV) | **Sentinel-1** (First available image for year 2020) |
| **2** | Radar Backscatter (VH) | **Sentinel-1** (First available image for year 2020) |
| **3** | Elevation | [NASA NASADEM](https://developers.google.com/earth-engine/datasets/catalog/NASA_NASADEM_HGT_001) |
| **4** | Slope | Derived from Elevation |
| **5** | Canopy Height (Target) | Global canopy height map for 2020 by [Lang et al. (2023)](https://doi.org/10.1038/s41559-023-02206-6) |

## Model training and evaluation

The dataset was partitioned into training (70%), validation (15%), and testing (15%) subsets. I benchmarked five distinct deep learning architectures: a basic U-Net, three U-Net variations with different backbones (MobileNetV2, EfficientNet-B1, and ResNet18), and a Mix Vision Transformer (MiT-b0).

All models were trained for 100 epochs using identical hyperparameters and regularization settings. To assess the impact of data processing strategies, each architecture was evaluated under four scenarios: (1) a baseline with no augmentation or transfer learning, (2) data augmentation only, (3) transfer learning (ImageNet weights) only, and (4) both augmentation and transfer learning. Performance across all experiments was measured using Mean Squared Error (MSE) and Root Mean Squared Error (RMSE).

## Results

Transfer learning had a more consistent decrease in loss when I did not use data augmentation. On the other hand, augmentation increased loss during training but consistently decreased loss during validation and testing. The best performing model was the **U-Net with a MobileNetV2 backbone**, using pre-trained ImageNet weights and data augmentation (random flips and radar noise). This configuration achieved the lowest error for the 100th epoch with a **Test RMSE of 7.38 meters**.

![Picture1](https://github.com/user-attachments/assets/f33025d9-6b88-49df-a05c-4a46a1bb3567)

## Reflection

The final RMSE of 7.38 meters is a surprisingly positive result given the constraints of this pilot study. While the error margin is large enough to prevent this specific model from being used for precise analyses, it is not exaggerated. The fact that the model generalized well suggests that using SAR (Sentinel-1) and topographic data holds valuable predictive power for canopy structure. However, several critical limitations in the data and methodology likely capped the model's potential performance.

The "Proxy" ground truth Problem was the most significant limitation, as I did not train or test the model on on-the-ground measurements or direct LiDAR point clouds. Instead, I used the global canopy height map by Lang et al. (2023) as my ground truth. Since the Lang map is itself a prediction generated based on GEDI and Sentinel-2 optical data, my project was essentially training a student model to mimic a teacher model, rather than learning from physical reality. Any errors or biases inherent in the Lang dataset were inevitably propagated into my results.

Additionally, I worked with only 999 tiles, which resulted in a very small dataset problem, even though data augmentation helped mitigate it to some extent. Furthermore, the tile dimension of 64x64 pixels may have been too restrictive. Canopy height in tropical forests is often influenced by larger landscape features—such as position on a watershed or mountain range—that require a wider field of view to understand. By cropping the world into small squares, I may have removed the broader contextual clues the CNN needed to make better predictions.

Finally, tree height is not just a function of terrain and roughness; it is biological. Factors such as soil composition, precipitation levels, and forest age are major drivers of vegetation growth, but none of these variables were included in the input channels.

Despite these limitations, the models successfully learned to extract spatial patterns consistent with the Lang et al. predictions. The final error rate is comparable to other studies, such as the findings by [Tolan et al. (2024)](https://doi.org/10.1016/j.rse.2023.113888) for Brazil, suggesting that the use of SAR and topographic data remains a viable approach for large-scale estimation

---
*Created by Daniela Linero for the AI for Earth Observation Final Project.*
