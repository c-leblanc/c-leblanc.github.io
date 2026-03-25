---
layout: single
title: "EOLES-Dispatch"
permalink: /eoles-dispatch/
author_profile: true
---
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-Z7QB0ZV44P"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-Z7QB0ZV44P');
</script>

<p>
  <a href="https://github.com/c-leblanc/EOLES-Dispatch" class="btn btn--primary"><i class="fab fa-github"></i> GitHub Repository</a>
</p>

EOLES-Dispatch is an open-source **electricity dispatch model** that simulates wholesale electricity prices at an hourly time-step for the French power system and six of its European neighbors. The model is a linear relaxation of a unit commitment problem: it minimizes total system dispatch cost subject to physical and operational constraints, and the **dual variable** of the supply-demand balance constraint yields the simulated spot price in each area and hour.

The model covers France, Belgium, Germany, Switzerland, Italy, Spain, and Great Britain as co-optimized areas, with eleven additional countries (Netherlands, Denmark, Sweden, Poland, Czech Republic, Austria, Greece, Slovenia, Portugal, Ireland) entering as exogenous price boundaries. This multi-country structure ensures that simulated French prices reflect realistic import/export patterns rather than an isolated system. Data is collected automatically from the ENTSO-E Transparency Platform, the Elexon BMRS (for GB post-Brexit), and Renewables.ninja.


Model Overview
------

The objective is to minimize total dispatch cost over all areas and hours. The **supply-demand adequacy constraint** is central: its dual variable gives the spot price. Key modeling elements include:

- **Thermal plants**: minimum stable generation, startup/shutdown dynamics (LP relaxation), ramping costs, part-load efficiency via a two-efficiency decomposition
- **Variable renewables**: generation bounded by capacity × hourly capacity factor (from Renewables.ninja or historical actuals)
- **Hydro and storage**: state-of-charge dynamics, monthly inflow balance for reservoirs, pumped-hydro constraints
- **Nuclear**: weekly availability factor caps from maintenance schedules
- **Cross-border trade**: bilateral flows bounded by interconnection capacity, 2% transmission losses
- **Reserves**: spinning reserve requirement sized on VRE installed capacity and demand uncertainty

The model runs at hourly resolution over up to a full year (8,760 hours) and is available in two variants: a *standard* model with full thermal dynamics, and a *static thermal* variant for faster computation.


Role in My Research
------

EOLES-Dispatch was developed as the quantitative backbone of my [PhD thesis](https://pastel.hal.science/tel-04269809) (*Microeconomic Analysis of Subsidy Mechanisms for Power Generation from Wind and Solar Sources*, École des Ponts ParisTech / CIRED, 2023) and of the associated research projects described below.

**Why a dispatch model?** A central challenge in analyzing renewable energy support mechanisms is that the value of a subsidy contract for a developer depends critically on the **sequence of spot prices** they face over the project lifetime. This sequence is not a primitive of the problem: it is itself shaped by the mix of generation technologies, fuel prices, weather conditions, and the contract designs in place. Structural analysis therefore requires a model that can generate realistic, internally consistent price paths — including their hourly and seasonal dynamics — under alternative scenarios and counterfactual policy configurations. Standard reduced-form price proxies are not sufficient for this purpose.

**Incentives, risk, and contract design.** My paper ["Renewables and Electricity Spot Prices: An incentive-risk trade-off for contract design"](http://c-leblanc.github.io/files/Contract_Design_Renewables_202312.pdf) ([slides](http://c-leblanc.github.io/files/Contract_Design_Renewables_slides_202310.pdf)) uses EOLES-Dispatch to quantify the two sides of the incentive-risk tradeoff that characterizes sliding feed-in premiums. On the one hand, exposing developers to spot prices preserves **locational and technological incentives**: firms value projects that produce when prices are high (typically during peak demand or low-wind periods), pushing investment toward high-value sites and technologies. On the other hand, this exposure creates **financial risk**: developers face revenue uncertainty that translates into risk premiums in competitive auctions. The model provides simulated hourly revenue streams for a sample of individual VRE projects — both wind and solar, at different locations across France — under a range of contract designs and fuel-price shock scenarios. A key finding is that, for the French power system in the late 2010s, the increase in risk premiums from moving to a pure market premium exceeds the welfare gains from better incentives by **an order of magnitude**. This asymmetry motivates the analysis of sliding-premium mechanisms that can mitigate risk while preserving incentives — and the paper identifies yearly price averaging as the relevant insurance horizon.

**CO2 externalities and project valuation.** The dispatch model also enables the computation of the **CO2 externality of individual projects**: by treating each project as a marginal change to installed capacity and computing the resulting shift in the system's dispatch cost (via the dual of the adequacy constraint), one can recover the shadow value of the emissions displaced by each project. This is used to assess whether contracts indexed to average electricity prices are a good proxy for social value — or whether they should be supplemented by an explicit carbon component.

**Counterfactual price scenarios.** More broadly, the model serves as a tool for generating **counterfactual price sequences** under alternative configurations of the power system — different capacity mixes, fuel price trajectories, or trade integration levels. This is useful both for retrospective policy evaluation and for prospective analysis of the energy transition.


Acknowledgements
------

EOLES-Dispatch builds on the [EOLES family of models](https://www.centre-cired.fr/the-eoles-family-of-models/) developed by Behrang Shirizadeh, Quentin Perrier, and Philippe Quirion at CIRED. The Python implementation was originally developed by Nilam De Oliveira-Gill. Renewable generation profiles are provided by Iain Staffell and Stefan Pfenninger via [Renewables.ninja](https://www.renewables.ninja/), and power system data comes from the [ENTSO-E Transparency Platform](https://transparency.entsoe.eu/).
