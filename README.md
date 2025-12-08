# Using radar and deep learning to estimate canopy height across the tropics

![Python](https://img.shields.io/badge/Python-3.x-blue?style=flat&logo=python)
![Framework](https://img.shields.io/badge/Framework-PyTorch-orange?style=flat&logo=pytorch)
![Topic](https://img.shields.io/badge/AI4EO-Earth%20Observation-green)

## Motivation
Tropical and Subtropical Moist Broadleaf Forests (TSMBF) are home to most of the world's known wild species and play a vital role in regulating the climate by storing large amounts of carbon. Earth Observation (EO) imagery has been key for monitoring the extent and state of these forests across large areas and over long periods. While optical satellite imagery has traditionally been the main source of data for such analyses, Synthetic Aperture Radar (SAR) imagery is increasingly important for its ability to penetrate the vertical structure of forests and capture details that optical sensors often miss. SAR is also especially useful in tropical regions, because it can collect data regardless of persistent cloud cover. These features make SAR data particularly well-suited for studying changes in TSMBF, as it is susceptible to structural attributes such as canopy height—a key biodiversity variable for remote sensing that serves as both an indicator of habitat heterogeneity for wildlife and a proxy for forest biomass and carbon stocks.

This repository was developed as the final project for the **AI for Earth Observation** course of the University of Wisconsin-Madison. 

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

## Results

## Reflection

---
*Created by [Daniela Linero] for the AI for Earth Observation Final Project.*
