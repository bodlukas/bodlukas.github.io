---
title: "ShakemapSim: Modelling conditional ground-motion intensity measures"
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

|------||------|
|<a class="btn btn--primary" href="https://github.com/bodlukas/ground-motion-simulation-shakemap"> <i class="fa fa-code" aria-hidden="true"></i> code</a>|<a class="btn btn--primary" href="https://doi.org/10.5281/zenodo.7646888"> <i class="fa fa-link fa-lg"></i> doi: 10.5281/zenodo.7646888 </a>|

**ShakemapSim** is a tool generate spatially correlated fields of ground-motion intensity measures (IMs) conditional on available recordings from a seismic network. This enables regional post-earthquake loss assessments and is particularly useful for validation and development of models for damage, loss and recovery predictions using data gathered after an event. The tool comes with rich documentation, is easily customizable and can be run on hosted Jupyter notebook services (i.e., Google Colab). 

We illustrate the tool using the [2023 M7.8 earthquake](https://earthquake.usgs.gov/earthquakes/eventpage/us6000jllz/executive) at the border of Turkey and Syria. Rupture information and station data (including recorded IMs) are imported from the [USGS ShakeMap system](https://earthquake.usgs.gov/data/shakemap/) and ShakemapSim uses the [OpenQuake engine](https://github.com/gem/oq-engine#openquake-engine) for geo-computations and implementations of ground-motion models. 

{% include figure image_path="/assets/images/research/ShakemapSim_fig1.png" alt="fault_turkeysyria" caption="Overview of the considered rupture (solid line), the hypocenter (star) and the seismic network stations (triangles) with recorded values for Sa(1s). All retrieved from the USGS ShakeMap for the 2023 M7.8 Central Turkey event (version 12). The region indicated with the shaded square will be the focus in the next section." %}

I developed the tool as part of my PhD in the research group of [Professor Stojadinovic](https://stojadinovic.ibk.ethz.ch/) at ETH Zürich. 

## Motivation
In February 2023, a strong M7.8 earthquake occurred at the border of Turkey and Syria resulting in the collapse of thousands of buildings with (too) many people that suffered from tragic losses. In the aftermath of such devastating events, people raise questions whether buildings were adequately designed and, even more, whether building code prescriptions for new constructions are sufficient.

Within days after the event, people posted on social media that at some seismic network stations the recorded ground-motion spectrum exceeded the design spectrum (from the building code) by a factor of two or more. Thus, suggesting that the observed collapses of new buildings may be attributed to poor building code prescriptions. But is that true?

There are at least two reasons why I think the above conclusions are premature and do not serve as an adequate explanation: First, modern building codes (such as the one in Turkey) embrace the philopophy of ductile design. Thus, the collapse of a code-compliant building is very unlikely even if the design spectra was exceeded. Second, the recorded ground-motion spectrum is only represenatative of a building located directly above the seismic network station. As pointed out by Peter Stafford in the aftermath of the 2011 Christchurch (New Zealand) earthquake, the ground-shaking experienced by other buildings is still highly uncertain, even if the buildings are located close to the seismic network station.

The ShakemapSim tool specifically adresses the second aspect mentioned above: It allows users to compute the probability distribution of ground-motion IMs at specified sites by taking into account recorded IM values at seismic network stations. 

## Example

**Quick start** Open the notebook [ShakemapSim_Example.ipynb](ShakemapSim_Example.ipynb) on a hosted Jupyter notebook service (e.g., Google Colab). It does not require any local python setup and you can immediately start to customize the models and perform the computations yourself. It explains how to: 
1. import earthquake rupture information and recorded ground-motion IMs,
2. specify (and customize) which ground-motion models and spatial correlation models are used to compute the shakemap,
3. specify sites at which we would like to predict ground-motion IMs,
4. use **ShakemapSim** to predict and sample ground-motion IMs. 

### Maps of conditional ground-motion IM parameters

{% include figure image_path="/assets/images/research/ShakemapSim_fig2.png" alt="map_hatay" caption="Maps of median predicted Sa(1s) values (left) and corresponding logarithmic standard deviation (right) conditional on seismic network recordings. Triangles indicate seismic network stations." %}

### Spatially correlated simulations of ground-motion IMs

{% include figure image_path="/assets/images/research/ShakemapSim_fig3.png" alt="sim_hatay" caption="Two spatially correlated samples of Sa(1s) at 2000 building sites in the city of Antakya conditional on recordings from seismic network stations (triangles)." %}

**Citation** <br /> Bodenmann, L., and Stojadinović, B. (2023): ShakemapSim: Simulate spatially correlated ground-motion intensity measures conditional on recordings (v1.1). <br /> <a class="btn btn--primary" href="https://doi.org/10.5281/zenodo.7646888"> <i class="fa fa-file-link fa-lg"></i> doi: 10.5281/zenodo.7646888 </a> <a class="btn btn--primary" href="https://github.com/bodlukas/ground-motion-simulation-shakemap"> <i class="fa fa-code" aria-hidden="true"></i> code</a>
{: .notice--info}
