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

{% include figure image_path="/assets/images/research/ShakemapSim_fig1.png" alt="fault_turkeysyria" caption="Overview of the considered rupture and the seismic network stations with recorded values for Sa(1s). Both were retrieved from the USGS ShakeMap for the 2023 M7.8 Central Turkey event (version 12)." %}



## Empirical models to simulate ground-motion IMs 

The following explanations are partially copied from our recent study, Bodenmann et al. (2023), and the notation is mostly based on the book of Baker et al. (2021). The latter is also an excellent resource for readers that want to know more about seismic hazard and risk analysis.

### Ground-motion models

We focus on empirically derived ground-motion models (GMMs) that predict a ground-motion IM at site $i$ induced by an earthquake rupture $k$ as

$$ \ln \textit{IM}_{ki} = \mu_{\ln IM}(rup_k, site_i; \psi_{\mathtt{GMM}}) + \delta B_k + \delta W_{ki}~, $$

where $\mu_{\ln \textit{IM}}(\cdot)$ is the predicted mean $\ln {\textit{IM}}$ value as a function of rupture ($rup$) and site ($site$) characteristics. Amongst others, these typically include earthquake magnitude, rupture mechanism, source-to-site distance and site-specific geological information. The parameters of the GMM are denoted as $\psi_{\mathtt{GMM}}$. The between-event and within-event residuals, $\delta B_k$ and $\delta W_{ki}$, are assumed to be independent, normally distributed variables with standard deviations $\tau$ and $\phi$, respectively. Empirical GMMs provide the mean function $\mu_{\ln \textit{IM}}(\cdot)$, as well as the standard deviations $\tau$ and $\phi$. For a specific event, the between-event residual denotes a common deviation from the predicted mean that is constant for all sites, whereas the within-event residuals vary in space. This is discussed in the following section. 

### Spatial correlation models

Conditional on rupture $rup_k$, the joint distribution of ground-motion IMs at $n$ spatially distributed sites $\mathbf{IM}_k=\(\textit{IM}_{k1},\ldots,\textit{IM}_{kn}\)^\top$ is commonly assumed to be a multivariate lognormal distribution 

$$ \ln{\mathbf{IM}_k} \sim \mathcal{N}(\boldsymbol{\mu},\tau^2 + \phi^2 \cdot \mathbf{C} )~, $$

where $\boldsymbol{\mu}$ is the mean vector with entries derived from the mean function $\mu_{\ln \textit{IM}}(\cdot)$ of the GMM, and $\mathbf{C}$ is the correlation matrix of the within-event residuals $\boldsymbol{\delta} \mathbf{W}_{k}$. To compute the entries of this matrix we employ a model $\rho(\cdot)$ that predicts the correlation between two sites $site_i$ and $site_j$ as 

$$ [\mathbf{C}]_{ij}=\rho(site_i,site_j; \psi_{\mathtt{SCM}})~, $$

where $\psi_{\mathtt{SCM}}$ denotes the spatial correlation model (SCM) parameters. Commonly, these models are defined for a distance metric $d$ between two sites. Thus, we often denote the correlation model as $\rho(d; \psi_{\mathtt{SCM}})$. The vast majority of SCMs proposed in the literature assume that correlation decreases exponentially with the Euclidean distance, $d_\mathrm{E}$, between two sites as

$$ \rho(d_\mathrm{E}; \psi_{\mathtt{SCM}})=\exp\left[- \left(\frac{d_\mathrm{E}}{\ell}\right)^\gamma\right]~, $$

where parameters $\psi_{\mathtt{SCM}}=(\ell,\gamma)$, denote the lengthscale and the exponent, respectively. 

Having computed the parameters of the multivariate normal distribution, sampling of $\ln im$ values is done by using methods implemented in other packages (such as `numpy`). 
