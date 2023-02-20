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
|<a class="btn btn--primary" href="https://github.com/bodlukas/ground-motion-simulation-shakemap"> <i class="fa fa-code" aria-hidden="true"></i> code</a>|<a class="btn btn--primary" href="https://doi.org/10.5281/zenodo.7646888"> <i class="fa fa-link fa-lg"></i> Zenodo doi </a>|

**ShakemapSim** is a tool generate spatially correlated fields of ground-motion intensity measures (IMs) conditional on available recordings from a seismic network. This enables regional post-earthquake loss assessments and is particularly useful for validation and development of models for damage, loss and recovery predictions using data gathered after an event. The tool comes with rich documentation, is easily customizable and can be run on hosted Jupyter notebook services (i.e., Google Colab). 

We illustrate the tool using the [2023 M7.8 earthquake](https://earthquake.usgs.gov/earthquakes/eventpage/us6000jllz/executive) at the border of Turkey and Syria. Rupture information and station data (including recorded IMs) are imported from the [USGS ShakeMap system](https://earthquake.usgs.gov/data/shakemap/) and ShakemapSim uses the [OpenQuake engine](https://github.com/gem/oq-engine#openquake-engine) for geo-computations and implementations of ground-motion models. 

{% include figure image_path="/assets/images/research/ShakemapSim_fig1.png" alt="fault_turkeysyria" caption="Overview of the considered rupture (solid line), the hypocenter (star) and the seismic network stations (triangles) with recorded values for Sa(1s). All retrieved from the USGS ShakeMap for the 2023 M7.8 Central Turkey event (version 12). The region indicated with the shaded square will be the focus in the next section." %}

I developed the tool as part of my PhD in the research group of [Professor Stojadinovic](https://stojadinovic.ibk.ethz.ch/) at ETH Zürich. 

## Motivation
In February 2023, a strong M7.8 earthquake occurred at the border of Turkey and Syria resulting in the collapse of thousands of buildings with (too) many people that suffered from tragic losses. In the aftermath of such devastating events, people raise questions whether buildings were adequately designed and, even more, whether building code prescriptions for new constructions are sufficient.

Within days after the event, people posted on social media that at some seismic network stations the recorded ground-motion spectrum exceeded the design spectrum (from the building code) by a factor of two or more. Thus, suggesting that the observed collapses of new buildings may be attributed to poor building code prescriptions. But is that true?

There are at least two reasons why I think the above conclusions are premature and do not serve as an adequate explanation: 
- First, modern building codes (such as the one in Turkey) embrace the philopophy of ductile design. Thus, the collapse of a code-compliant building is very unlikely even if the design spectra was exceeded. 
- Second, the recorded ground-motion spectrum is only represenatative of a building located directly above the seismic network station. The ground-shaking experienced by other buildings is still highly uncertain, even if the buildings are located close to a seismic network station. This was also pointed out by Peter Stafford after the 2011 Christchurch (New Zealand) earthquake[^Stafford2012].

The ShakemapSim tool adresses the second aspect mentioned above: It allows users to compute the probability distribution of ground-motion IMs at specified building locations by taking into account recorded IM values at seismic network stations. 

Conducting, validating and developing regional post-earthuquake loss assessments requires correlated samples of ground-motion IMs at spatially distributed locations. These samples should be representative of the remaining uncertainty in ground-motions after accounting for recorded IM values. The following example illustrates the capabilities of ShakemapSim.

## Example

The following example illustrates the capabilities of ShakemapSim in producing maps of conditional ground-motion IM parameters and generating correlated samples at specified building sites. 

The code snippet below shows how to specify and assemble the different model components. 

```python
# 1) Specify IM of interest
im_string = 'SA(1.0)'

# 2) Specify ground-motion model (GMM)
gmm = GMM(CauzziEtAl2014(), im_string)

# 3) Specify spatial correlation model (SCM)
scm = EspositoIervolino2012esm(im_string)

# 4) Wrap seismic network stations
stations = Stations(dfstations, im_string)

# 5) Assemble shakemap object
shakemap = Shakemap(Rupture = rupture, 
                    Stations = stations,
                    GMM = gmm,
                    SCM = scm)
```

The specification of building sites for which we would like to conduct predictions and/or sampling is straightforward.

```python
# Specify sites (lon, lat in degrees; vs30 in m/s)
dfsites = pd.DataFrame(data={'longitude': [36.51, 36.62], 
                             'latitude': [37.01, 37.10],
                             'vs30': [210, 500]})
sites = Sites(dfsites)
```

### Maps of conditional ground-motion IM parameters

One line of code is enough to predict the probability distribution of ground-motion IMs conditional recorded IM values at specified sites.  
```python
mu, var = shakemap.predict_logIM(sites)
```
The following maps show the parameters of the predicted lognormal distribution (left: median, right: standard deviation) at a longitude/latitude grid in the Turkish region of Hatay. 

{% include figure image_path="/assets/images/research/ShakemapSim_fig2a.png" alt="map_hatay" caption="Maps of median predicted Sa(1s) values (left) and corresponding logarithmic standard deviation (right) conditional on seismic network recordings. Triangles indicate seismic network stations. The region indicated with the shaded square in the right panel will be the focus in the next section. The two sites indicated in the right panel are plotted in the figure below." %}

For two sites, we plot the corresponding marginal logarithmic pdf. 

| Distances to three closest stations in km | | | |
| :---        |    ---:   |    ---:   |    ---:   |
| Site 1 | 7.7km | 10.1km | 10.3km |
| Site 2 | 0.8km | 1.1km | 1.8km |

{% include figure image_path="/assets/images/research/ShakemapSim_fig3.png" alt="pdf_hatay" caption="Probability density function of Sa(1s) at two sites conditional on seismic network recordings." %}

### Spatially correlated samples of ground-motion IMs

Next we generate two spatially correlated samples of IMs conditional on recorded IM values at locations of 2000 buildings in the city of Antakya (Hatay, Turkey) using following line of code:
```python
sampled_logIM = shakemap.sample_logIM(sites, nsamples = 2)
```

{% include figure image_path="/assets/images/research/ShakemapSim_fig3.png" alt="sim_hatay" caption="Two spatially correlated samples of Sa(1s) at 2000 building sites in the city of Antakya conditional on recordings from seismic network stations (triangles)." %}

For regional damage, loss and recovery studies, it is of interest to statistically estimate the number of building sites where the IM of interest exceeded a given threshold.

For the same 2000 building sites, we will now compute the probability distribution of the proportion of sites where the IM of interest exceeded a certain threshold. 

First, we sample a large number (10000 in this example) of ground-motion fields (like those in the figure above). Then, for each sample, we count the number of sites where the IM of interest threshold was exceeded and divide by the total number of sites to get the proportion of sites. 

In this example, the IM of interest is SA(1.0) (as defined above) and its threshold is set to 0.75g.

{% include figure image_path="/assets/images/research/ShakemapSim_fig4.png" alt="hist_hatay" caption="For the 2000 building sites from above: Histogram of the proportion of sites where Sa(1s) exceeded a threshold of 0.75g. The histogram was obtained from 10000 simulated ground-motion fields." %}

> **_Note:_** These findings are for the selected region, for the IM SA(1s), an IM threshold of 0.75g, as well as for one specific ground-motion model and one specific spatial correlation model. Feel free to re-run the [code](https://github.com/bodlukas/ground-motion-simulation-shakemap) using the different implemented models (or implement your own models), and vary the region, IM and IM threshold values. 

**Citation** <br /> Bodenmann, L., and Stojadinović, B. (2023): ShakemapSim: Simulate spatially correlated ground-motion intensity measures conditional on recordings (v1.1). <br /> <a class="btn btn--primary" href="https://doi.org/10.5281/zenodo.7646888"> <i class="fa fa-file-link fa-lg"></i> doi: 10.5281/zenodo.7646888 </a> <a class="btn btn--primary" href="https://github.com/bodlukas/ground-motion-simulation-shakemap"> <i class="fa fa-code" aria-hidden="true"></i> code</a>
{: .notice--info}

{% include references.md %}
