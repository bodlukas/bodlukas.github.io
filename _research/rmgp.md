---
title: "Dynamic updating of post-earthquake loss estimates"
permalink: /research/rmgp/
author_profile: false
comments: false
layout: single
date: 2023-01-15
toc: true
toc_label: "Content"
toc_icon: "bars"  
toc_sticky: true  
---

The widespread earthquake damage to the built environment induces severe short- and long-term societal consequences. Better community resilience may be achieved through well-organized recovery. Decisions to organize the recovery process are taken under intense time pressure using limited, and potentially inaccurate, data on the severity and the spatial distribution of building damage. 

In this work, we suggest to encode domain knowledge in ordinal Gaussian process regression to update parts of the regional earthquake risk model using early damage reports from building inspection. The updated model is then used to issue post-earthquake loss estimates with increasing accuracy and precision. 

**More information** <br /> Bodenmann L., Reuland Y. and Stojadinović B. (2022): Dynamic post-earthquake updating of regional damage estimates using Gaussian processes; Submitted to Reliability Engineering & System Safety. <br /> <a class="btn btn--primary" href="https://doi.org/10.31224/2205"> <i class="fa fa-file-pdf fa-lg"></i> Preprint at engXriv</a> <a class="btn btn--primary" href="https://github.com/bodlukas/earthquake-rmgp"> <i class="fa fa-code" aria-hidden="true"></i> code</a>
{: .notice--info}

## Regional earthquake risk model

The following contains a simplified summary of the typical workflow for such regional analyses. More detailed descriptions can be found in the excellent book by Baker, Bradley and Stafford[^Baker2021], and in the highly recommended report by Deierlein and Zsarnóczay[^SimCenter2021].

{% include figure image_path="/assets/images/research/rmgp_rmonly.png" alt="schema_risk_model" caption="Simplified workflow of a regional earthquake risk model." %}

The **asset model** assembles the necessary information about the system of interest. In our case this is the building inventory of a specific region. The information pertains to the geographic locations of the individual buildings, their age, and some information on their geometries (such as their height). When information is missing, the asset model specifies how we estimate missing information for the subsequent computations. 

The **source model** consists of the characterization of potential earthquake ruptures that may affect the region of interest. It is typically derived via statistical analyses of historical earthquake events in combination with physical constraints and geodesic measurements. 

The **ground-motion model** predicts the level of ground-shaking at the surface induced by a specific earthquake rupture. Typically, ground-shaking is expressed via intensity measures such as peak ground acceleration. There exist empirical and physics-based ground-motion models that provide probabilistic estimates of such intensity measures at the geographic locations of interest. Here we focus on empirical models, which model the spatial distribution of the intensity measure(s) as a stochastic process (also called random field). In the linked paper, we explain how such models can be expressed as Gaussian processes (GPs).

The **fragility model** estimates the building damage as a result of a certain level of ground-shaking. Such models are derived via high-fidelity structural models of archetype buildings, and empirically using damage data from past earthquakes. In the following, we assume that building damage is described via discrete categories (e.g., none, slight, ..., complete), so-called damage states.

The **loss model** computes system-wide losses as a result of the individual component damage states. For buildings, one could model the number of injured persons for each building as a function of its damage state and then compute the total number of injured persons by aggregating over all buildings. Similar computations may be done for direct financial losses (i.e., repair costs). 

## The post-earthquake situation

In the immediate aftermath of an earthquake, it is imperative to use the early-arriving shaking intensity and building damage data as soon as possible to constrain the uncertainty of regional damage estimates and to organize recovery. Below, I explain the inference steps that use the ground motion intensity data measured by seismic networks, and the building damage and typological data obtained by inspection.

{% include figure image_path="/assets/images/research/rmgp_github_bright.png" alt="schema_rmgp" caption="Schema of the proposed updating of a regional earthquake risk model with early-arriving post-earthquake data." %}

Within minutes after the event, ground-motion recordings from a **seismic network** become available. These are used to: _(i)_ determine characteristics of the occurred rupture (such as its magnitude, location and extent), and _(ii)_ constrain ground-motion estimates (analogous to current [shake map](https://earthquake.usgs.gov/data/shakemap/) systems). Because the spatial distribution of (logarithmic) intensity measures can be expressed as a GP, step _(ii)_ is essentially equivalent to GP regression[^GPML2006]. 

Then, first damage estimates are obtained by including the determined rupture characteristics and the constrained ground-motion estimates in the regional risk model.

Within hours and days after the event, public agencies and volunteers start with **building inspection**. Through visual inspection, experts are asked to assess the building condition and whether a damaged building is safe enough to be re-occupied by its residents. 

Typically, the inflicted damage is stated in terms of a certain damage category. Here we assume that those categories are consistent with the damage states employed in the fragility model of the risk model. Besides this damage description, experts also provide some basic building attributes that were not known prior to the event, e.g., the dominating material of the lateral load-resisting system and its type(REF). 

While completing the entire inspection campaign may take weeks to months, our proposed framework uses data from the first inspected buildings to update (i) the shake map, (ii) the fragility model, and (iii) the typological attribution process of the asset model. 
The gathered attributes, together with the information available in the asset model, allows an allocation of the 

## Case studies
We tested the proposed framework in three case-studies (see figure below). I would to illustrate some key characteristics ... 

## Summary
coming soon ...


{% include references.md %}
