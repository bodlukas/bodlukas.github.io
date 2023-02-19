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

$$ \ln \textit{IM}_{ki} = \mu_{\ln IM}(rup_k, site_i; \psi_{\mathtt{GMM}}) + \delta B_k + \delta W_{ki}~, $$

where $\mu_{\ln \textit{IM}}(\cdot)$ is the predicted mean $\ln {\textit{IM}}$ value as a function of rupture ($rup$) and site ($site$) characteristics. Amongst others, these typically include earthquake magnitude, rupture mechanism, source-to-site distance and site-specific geological information. The parameters of the GMM are denoted as $\psi_{\mathtt{GMM}}$. The between-event and within-event residuals, $\delta B_k$ and $\delta W_{ki}$, are assumed to be independent, normally distributed variables with standard deviations $\tau$ and $\phi$, respectively. Empirical GMMs provide the mean function $\mu_{\ln \textit{IM}}(\cdot)$, as well as the standard deviations $\tau$ and $\phi$. For a specific event, the between-event residual denotes a common deviation from the predicted mean that is constant for all sites, whereas the within-event residuals vary in space. This is discussed in the following section. 

### Spatial correlation models

Conditional on rupture $rup_k$, the joint distribution of ground-motion IMs at $n$ spatially distributed sites $\mathbf{IM}_k=(\textit{IM}_{k1},\ldots,\textit{IM}_{kn})^\top$ is commonly assumed to be a multivariate lognormal distribution 

$$ \ln{\mathbf{IM}_k} \sim \mathcal{N}(\boldsymbol{\mu},\tau^2 + \phi^2 \cdot \mathbf{C} )~, $$

where $\boldsymbol{\mu}$ is the mean vector with entries derived from the mean function $\mu_{\ln \textit{IM}}(\cdot)$ of the GMM, and $\mathbf{C}$ is the correlation matrix of the within-event residuals $\boldsymbol{\delta} \mathbf{W}_{k}$. To compute the entries of this matrix we employ a model $\rho(\cdot)$ that predicts the correlation between two sites $site_i$ and $site_j$ as 

$$ [\mathbf{C}]_{ij}=\rho(site_i,site_j; \psi_{\mathtt{SCM}})~, $$

where $\psi_{\mathtt{SCM}}$ denotes the spatial correlation model (SCM) parameters. Commonly, these models are defined for a distance metric $d$ between two sites. Thus, we often denote the correlation model as $\rho(d; \psi_{\mathtt{SCM}})$. The vast majority of SCMs proposed in the literature assume that correlation decreases exponentially with the Euclidean distance, $d_\mathrm{E}$, between two sites as

$$ \rho(d_\mathrm{E}; \psi_{\mathtt{SCM}})=\exp\left[- \left(\frac{d_\mathrm{E}}{\ell}\right)^\gamma\right]~, $$

where parameters $\psi_{\mathtt{SCM}}=(\ell,\gamma)$, denote the lengthscale and the exponent, respectively. 

Having computed the parameters of the multivariate normal distribution, sampling of $\ln im$ values is done by using methods implemented in other packages (such as `numpy`). 
