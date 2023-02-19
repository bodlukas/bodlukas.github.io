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

## Empirical models to simulate ground-motion IMs 

The following explanations are partially copied from our recent study, Bodenmann et al. (2023), and the notation is mostly based on the book of Baker et al. (2021). The latter is also an excellent resource for readers that want to know more about seismic hazard and risk analysis.

### Ground-motion models

We focus on empirically derived ground-motion models (GMMs) that predict a ground-motion IM at site $i$ induced by an earthquake rupture $k$ as

$$ \ln \textit{IM}_{ki} = \mu_{\ln IM}(rup_k, site_i; \psi}{\mathtt{GMM}}) + \delta B_k + \delta W_{ki}~, $$

where $\mu_{\ln \textit{IM}}(\cdot)$ is the predicted mean $\ln {\textit{IM}}$ value as a function of rupture ($rup$) and site ($site$) characteristics. Amongst others, these typically include earthquake magnitude, rupture mechanism, source-to-site distance and site-specific geological information. The parameters of the GMM are denoted as $\psi_{\mathtt{GMM}}$. The between-event and within-event residuals, $\delta B_k$ and $\delta W_{ki}$, are assumed to be independent, normally distributed variables with standard deviations $\tau$ and $\phi$, respectively. Empirical GMMs provide the mean function $\mu_{\ln \textit{IM}}(\cdot)$, as well as the standard deviations $\tau$ and $\phi$. For a specific event, the between-event residual denotes a common deviation from the predicted mean that is constant for all sites, whereas the within-event residuals vary in space. This is discussed in the following section. 

> **_Note:_** We use GMM implementations from OpenQuake. Currently we only used the models `AkkarEtAl2014Rjb` and `CaucciEtAl2014`, with each having 35% weight in the USGS logic tree used for the M7.8 earthquake at the border of Turkey and Syria. Other models can easily be added by following the OpenQuake documentation. However, some models may require additional site and/or rupture characteristics than the ones considered so far.

### Spatial correlation models

Conditional on rupture $rup_k$, the joint distribution of ground-motion IMs at $n$ spatially distributed sites $\mathbf{IM}_k=(\textit{IM}_{k1},\ldots,\textit{IM}_{kn})^\top$ is commonly assumed to be a multivariate lognormal distribution 

$$ \ln{\mathbf{IM}_k} \sim \mathcal{N}(\boldsymbol{\mu},\tau^2 + \phi^2 \cdot \mathbf{C} )~, $$

where $\boldsymbol{\mu}$ is the mean vector with entries derived from the mean function $\mu_{\ln \textit{IM}}(\cdot)$ of the GMM, and $\mathbf{C}$ is the correlation matrix of the within-event residuals $\boldsymbol{\delta} \mathbf{W}_{k}$. To compute the entries of this matrix we employ a model $\rho(\cdot)$ that predicts the correlation between two sites $site_i$ and $site_j$ as 

$$ [\mathbf{C}]_{ij}=\rho(site_i,site_j; \psi}{\mathtt{SCM}})~, $$

where $\boldsymbol{\psi}_{\mathtt{SCM}}$ denotes the spatial correlation model (SCM) parameters. Commonly, these models are defined for a distance metric $d$ between two sites. Thus, we often denote the correlation model as $\rho(d; \boldsymbol{\psi}_{\mathtt{SCM}})$. The vast majority of SCMs proposed in the literature assume that correlation decreases exponentially with the Euclidean distance, $d_\mathrm{E}$, between two sites as
$$
\rho(d_\mathrm{E}; \boldsymbol{\psi}_{\mathtt{SCM}})=\exp\left[- \left(\frac{d_\mathrm{E}}{\ell}\right)^\gamma\right]~,
$$
where parameters $\boldsymbol{\psi}_{\mathtt{SCM}}=(\ell,\gamma)$, denote the lengthscale and the exponent, respectively. 

Having computed the parameters of the multivariate normal distribution, sampling of $\ln im$ values is done by using methods implemented in other packages (such as `numpy`). 

> **_Note:_** Currently we implemented following models: `EspositoIervolino2012esm`, `HeresiMiranda2019` and `BodenmannEtAl2023`. The first two models depend only on $d_\mathrm{E}$, while the last model additionally accounts for site and path effects, and thus also depends on the earthquake rupture. You find all references in the documentation of the [source files](modules/spatialcorrelation.py) which should also help you to easily add other SCMs.
