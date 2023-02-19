---
title: "Hindcasting ground-motion amplitudes after an event"
permalink: /research/shakemapsim/
author_profile: false
comments: false
layout: single
date: 2023-02-19
toc: true
toc_label: "Content"
toc_icon: "bars"  
toc_sticky: true 
---

ShakemapSim is a user-friendly tool to generate spatially correlated fields of ground-motion intensity measures (IMs) conditional on available recordings from a seismic network. This is particularly useful for validation and development of models for damage, loss and recovery predictions using data gathered after an event. 

If you use this tool in your work please please cite it as:
> Bodenmann, Lukas, and Stojadinović, Božidar. (2023). ShakemapSim: Simulate spatially correlated ground-motion intensity measures conditional on recordings (v1.0). Zenodo. https://doi.org/10.5281/zenodo.7646888

**Quick start** Open the notebook [ShakemapSim_Example.ipynb](ShakemapSim_Example.ipynb) on a hosted Jupyter notebook service (e.g., Google Colab). It does not require any local python setup and you can immediately start to customize the models and perform the computations yourself. It explains how to: 
1. import earthquake rupture information and recorded ground-motion IMs,
2. specify (and customize) which ground-motion models and spatial correlation models are used to compute the shakemap,
3. specify sites at which we would like to predict ground-motion IMs,
4. use **ShakemapSim** to predict and sample ground-motion IMs. 

The tool uses the [openquake engine](https://github.com/gem/oq-engine#openquake-engine) for geo-computations and implementations of ground-motion models. In the provided example we import rupture information and station data (including recorded IMs) from the [USGS shakemap system](https://earthquake.usgs.gov/data/shakemap/). 

The tool was developed by the research group of [Prof. Bozidar Stojadinovic](https://stojadinovic.ibk.ethz.ch/) at the Department of Civil, Environmental and Geomatic Engineering at ETH Zürich. 

## Overview: Earthquake rupture and seismic network stations

{% include figure image_path="/assets/images/research/ShakemapSim_fig1.png" alt="fault_turkeysyria" caption="Overview of the considered rupture (solid line), the hypocenter (star) and the seismic network stations (triangles) with recorded values for Sa(1s). All retrieved from the USGS ShakeMap for the 2023 M7.8 Central Turkey event (version 12). The region indicated with the shaded square will be the focus in the next section." %}

## Maps of conditional ground-motion IM parameters



## Spatially correlated simulations of ground-motion IMs

