# Using radar and deep learning to estimate canopy height across the tropics

![Python](https://img.shields.io/badge/Python-3.x-blue?style=flat&logo=python)
![Framework](https://img.shields.io/badge/Framework-PyTorch%20%7C%20TensorFlow-orange?style=flat)
![Topic](https://img.shields.io/badge/AI4EO-Earth%20Observation-green)

## Motivation
Tropical and Subtropical Moist Broadleaf Forests (TSMBF) are home to most of the world's known wild species and play a vital role in regulating the climate by storing large amounts of carbon. Earth Observation (EO) imagery has been key for monitoring the extent and state of these forests across large areas and over long periods. While optical satellite imagery has traditionally been the main source of data for such analyses, Synthetic Aperture Radar (SAR) imagery is increasingly important for its ability to penetrate the vertical structure of forests and capture details that optical sensors often miss. SAR is also especially useful in tropical regions, because it can collect data regardless of persistent cloud cover. These features make SAR data particularly well-suited for studying changes in TSMBF, as it is susceptible to structural attributes such as canopy height—a key biodiversity variable for remote sensing that serves as both an indicator of habitat heterogeneity for wildlife and a proxy for forest biomass and carbon stocks.

This repository was developed as the final project for the **AI for Earth Observation** course of the University of Wisconsin-Madison. 

## Research Questions

1.  How well do different deep learning architectures predict canopy height from radar and topographic data across the TSMBF?
2.  How do transfer learning and data augmentation affect model accuracy?
3.  Which architecture–training strategy combination achieves the lowest prediction loss?

---
*Created by [Daniela Linero] for the AI for Earth Observation Final Project.*
