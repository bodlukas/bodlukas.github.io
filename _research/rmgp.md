---
title: "Dynamic updating of post-earthquake loss estimates"
permalink: /research/rmgp/
author_profile: false
comments: false
layout: single
date: 2023-01-15
---

UNDER DEVELOPMENT

The widespread earthquake damage to the built environment induces severe short- and long-term societal consequences. Better community resilience may be achieved through well-organized recovery. Decisions to organize the recovery process are taken under intense time pressure using limited, and potentially inaccurate, data on the severity and the spatial distribution of building damage. 

There exists a wide range of possible measures and actions to make communities more resilient towards such extreme events. These include the construction of new flood protection systems or increasing the resistance of existing buildings to high wind loads and earthquake-induced ground-shaking. 

Apart from such structural mitigation measures, effective disaster response and recovery is crucial to reduce negative mid- and long-term consequences. 

In this work, we suggest to encode domain knowledge in ordinal Gaussian process regression to update parts of the regional earthquake risk model using early damage reports from building inspection. The updated model is then used to issue post-earthquake loss estimates with increasing accuracy and precision. 

## Workflow for regional earthquake risk analysis

{% include figure image_path="/assets/images/research/rmgp_rmonly.png" alt="schema_risk_model" caption="Simplified workflow of a regional earthquake risk model." %}

The **asset model** assembles the necessary information about the system of interest. In our case this is the building inventory of a specific region. The information pertains to the geographic locations of the individual buildings, their age, and some information on their geometries (such as their height). When information is missing the asset model specifies how we attribute missing information for the subsequent computations.

The **source model** consists of the characterization of potential earthquake ruptures that may affect the region of interest. It is primarily derived from earthquake catalogues. 

The **ground-motion model** predicts the level of ground-shaking at the surface induced by a specific earthquake rupture. Typically, ground-shaking is expressed via intensity measures such as peak ground acceleration. There exists empirical and physics-based ground-motion models. 

The **fragility model** estimates the building damage as a result of a certain level of ground-shaking. Analytically and empirically derived fragility functions ...

The **loss model** computes system-wide losses as a result of the individual component damage states. ... 

Seismic risk is then computed ... 

## The post-earthquake situation

...

{% include figure image_path="/assets/images/research/rmgp_github_bright.png" alt="schema_rmgp" caption="Schema of the proposed updating of a regional earthquake risk model with early-arriving post-earthquake data." %}

The **seismic recordings** ... 

The **building inspections** typically consist of (i) an estimate of the inflicted damage via discrete categories, so-called damage states, and (ii) decriptions of the type and material of the building examined. We use the first and second information to update the fragility model and the ground-motion model. The second information also allows us to improve our knowledge for the typological attribution performed in the asset model. 

## The updating workflow

Gaussian process ... ... $\mathbf{f}\propto \mathcal{GP}(m(x),k(x,x)$ bla bla 

\begin{align}
  V_{sphere} = \frac{4}{3}\pi r^3 \label{eq:test1}
\end{align}

## Case studies

Zurich and Pollino

...

## Summary

**More information** <br /> Bodenmann L., Reuland Y. and Stojadinovic B. (2022): Dynamic post-earthquake updating of regional damage estimates using Gaussian processes; Submitted to Reliability Engineering & System Safety. <br /> <a class="btn btn--primary" href="https://doi.org/10.31224/2205"> <i class="fa fa-link"></i> doi</a> <a class="btn btn--primary" href="https://engrxiv.org/preprint/download/2205/4410/3347"> <i class="fa fa-file-pdf fa-lg"></i> preprint</a> <a class="btn btn--primary" href="https://github.com/bodlukas/earthquake-rmgp"> <i class="fa fa-code" aria-hidden="true"></i> code</a>
{: .notice--info}


