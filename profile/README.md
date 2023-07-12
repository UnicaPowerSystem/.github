## Hey, this is the Power System Group from University of Cagliari 👋
[![Contributors][contributors-shield1]][contributors-url1]
[![Contributors][contributors-shield2]][contributors-url2]
[![Contributors][contributors-shield3]][contributors-url3]
[![MIT License][license-shield]][license-url]
[![LinkedIn 1st Author][linkedin-shield1]][linkedin-url-1st]
[![LinkedIn 2nd Author][linkedin-shield2]][linkedin-url-2nd]
[![LinkedIn 3rd Author][linkedin-shield3]][linkedin-url-3rd]



<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/MarcoGalici/MKT_Comparison/">
    <img src="figures/Logo_Università_di_Cagliari.jpg" alt="Logo" width="400" height="150">
  </a>

<h3 align="center">Market Pricing and Clearing Comparison - Comillas / UniCa</h3>

  <p align="center">
    Comparison of Nodal Pricing, Auction and Double Auction market with complex bids for Local Electricity Markets
    <br />
    <a href="https://github.com/MTr87/MKT_Comparison/tree/main/docs"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/github_username/repo_name">View Demo</a>
    ·
    <a href="https://github.com/MTr87/MKT_Comparison/issues">Report Bug</a>
    ·
    <a href="https://github.com/github_username/repo_name/issues">Request Feature</a>
  </p>
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li><a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
      <li><a href="#mathematical-formulation">The Optimisation Problems</a>
      <ul>
        <li><a href="#the-distribution-locational-marginal-price-model">Distribution Locational Marginal Price Model</a></li>
        <li><a href="#the-storage-system-model">Storage System Model</a></li>
        <li><a href="#the-auction-model-of-the-complex-bids-market">Auction Model</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#roadmap">Roadmap</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Project

<!--[![Product Name Screen Shot][product-screenshot]](https://example.com)-->

<!-- Here's a blank template to get started: To avoid retyping too much info. Do a search and replace with your text editor for the following: `github_username`, `repo_name`, `twitter_handle`, `linkedin_username`, `email_client`, `email`, `project_title`, `project_description`-->
<!-- Link for emoji https://www.webfx.com/tools/emoji-cheat-sheet/ -->

Nowadays, renewable energy resources are highly developed, and market with flexible combination of various types of energy have great coordination among power supply, grid, load and energy storage. This requires the design of various types of transactions and bidding forms with flexibility to meet the needs of different market participants, so that they can do some trading flexibly according to their own physical and economic characteristics.
Complex bids are an ideal schema for these problems 😃.


<!-- Built With -->
### Built With
* [![Python][Python-shield]][Python-url]
* [![Gurobi][Gurobi-shield]][Gurobi-url]

<!-- <p align="right"><a href="#top">🔼 Back to top</a></p> -->

<!-- Mathematical Problems -->
## Mathematical Formulation

<!-- Distribution Locational Marginal Price Model -->
### The Distribution Locational Marginal Price Model
```math
\max_{\Omega} \quad { \sum_{t=1}^{T} (\sum_{i=1}^{N_{bus}} \lambda^{d,P}_{t} \cdot P^{d}_{i,t} + \lambda^{d,Q}_{t} \cdot [Q^{d}_{i,t} - Q^{g}_{i,t}] + \Pi^{g}_{t} \cdot P^{g}_{i,t}) }
```
```math
\textrm{Where: }
```
```math
\quad {\Omega = \{P^d_{i,t}, P^g_{i,t}, Q^{d}_{i,t}, Q^{g}_{i,t}, SoC_{w,t}, P_{w,t}, Q_{w,t}, P_{z,t}, Q_{z,t}, P_{f,t}, Q_{f,t}, P^{cut}_{z,t}, P^{rb}_{z,t}, \Delta P_{i,t}, \Delta Q_{i,t}, P^{bus}_{i,t}, Q^{bus}_{i,t}\}}
```
```math
\textrm{s.t.} \quad {\Delta P_{i,t} = P^{DA}_{i,t} - P^{bus}_{i,t}} \quad \forall i \in N_{bus} \quad \forall t \in T
```
```math
\quad {P^{bus}_{i,t} = P_{Ess(i),t} + P_{Load(i),t} + P_{Gen(i),t}} \quad \forall i \in N_{bus} \quad \forall t \in T
```
```math
\quad {P^{bus}_{i,t} = P^{d}_{i,t} + P^{g}_{i,t}} \quad \forall i \in N_{bus} \quad \forall t \in T
```
```math
\quad {\Delta Q_{i,t} = Q^{DA}_{i,t} - Q^{bus}_{i,t}} \quad \forall i \in N_{bus} \quad \forall t \in T
```
```math
\quad {Q^{bus}_{i,t} = Q_{Ess(i),t} + Q_{Load(i),t} + Q_{Gen(i),t}} \quad \forall i \in N_{bus} \quad \forall t \in T
```
```math
\quad {Q^{bus}_{i,t} = Q^{d}_{i,t} + Q^{g}_{i,t}} \quad \forall i \in N_{bus} \quad \forall t \in T
```
```math
\quad {V^{min} \leq V^{DA}_{i,t} + \sum_{j=1}^{N_{bus}} \{ ({\partial P \over \partial V})^{-1}_{i,j} \cdot \Delta P_{j,t} + ({\partial Q \over \partial V})^{-1}_{i,j} \cdot \Delta Q_{j,t} \} \leq V^{max}} \quad \forall i \in N_{bus} \quad \forall t \in T
```
```math
\quad {\sum_{i=1}^{N_{bus}} \{ ( PTDF_{i,k} \cdot P^{bus}_{i,t} )^2 + (QTDF_{i,k} \cdot Q^{bus}_{i,t})^2 \} \leq S^2_k} \quad \forall k \in N_{Line} \quad \forall t \in T
```
```math
\quad \Biggl\{
\begin{array}{l}
  {SoC_{w,t} = SoC_{w,t-1} + \{\eta_{trip} \cdot P_{w,t} - (1 - \eta_{Q_{inv}}) \cdot | Q_{w,t} | \} \cdot \Delta t} \quad \textrm{if} \quad t \neq 1\\
  {SoC_{w,t} = SoC^{DA}_{w,0} + \{\eta_{trip} \cdot P_{w,t} - (1 - \eta_{Q_{inv}}) \cdot | Q_{w,t} | \} \cdot \Delta t} \quad \textrm{if} \quad t = 1
\end{array} \quad \forall w \in N_{Ess} \quad \forall t \in T
```
```math
\quad {SoC^{min}_{w} \leq SoC_{w,t} \leq SoC^{max}_{w}} \quad \forall w \in N_{Ess} \quad \forall t \in T
```
```math
\quad {-S_{w} \leq P_{w,t} \leq S_{w}} \quad \forall w \in N_{Ess} \quad \forall t \in T
```
```math
\quad {-S_{w} \leq Q_{w,t} \leq S_{w}} \quad \forall w \in N_{Ess} \quad \forall t \in T
```
```math
\quad {P^{2}_{f,t} + Q^{2}_{f,t} \leq S^{2}_{f}} \quad \forall f \in N_{Gen} \quad \forall t \in T
```
```math
\quad {P_{f,t} \cdot \tan(\arccos(pf^{max}_{f}) \leq Q_{f,t} \leq P_{f,t} \cdot \tan(\arccos(pf^{min}_{f}))} \quad \forall f \in N_{Gen} \quad \forall t \in T
```
```math
\quad {0 \leq P_{f,t} \leq P^{DA}_{f,t}} \quad \forall f \in N_{Gen} \quad \forall t \in T
```
```math
\quad {P_{z,t} = P^{DA}_{z,t} - P^{cut}_{z,t} + P^{rb}_{z,t}} \quad \forall z \in N_{Load} \quad \forall t \in T
```
```math
\quad {P^{rb}_{z,t} = \sum_{\tau = t-1}^{t-\delta} r_{z,\tau} \cdot P^{cut}_{z,\tau}} \quad \forall z \in N_{Load} \quad \forall t \in T
```
```math
\quad {0 \leq P^{cut}_{z,t} \leq P^{DA}_{z,t} + \sum_{\tau = t-1}^{t-\delta} r_{z,\tau} \cdot P^{cut}_{z,\tau}} \quad \forall z \in N_{Load} \quad \forall t \in T
```
```math
\quad {0 \leq P^{rb}_{z,t} \leq \sum_{\tau = t-1}^{t-\delta} r_{z,\tau} \cdot P^{DA}_{z,\tau}} \quad \forall z \in N_{Load} \quad \forall t \in T
```
```math
\quad {0 \leq \sum_{t=1}^{T} P^{rb}_{z,t} \leq \sum_{t=1}^{T} P^{cut}_{z,t}} \quad \forall z \in N_{Load}
```
```math
\quad {P_{z,t} \cdot \tan(\arccos(pf^{max}_{z})) \leq Q_{z,t} \leq P_{z,t} \cdot \tan(\arccos(pf^{min}_{z}))} \quad \forall z \in N_{Load} \quad \forall t \in T
```
```math
\quad {r_{z,t}} \Biggl\{
\begin{array}{l}
  {\alpha_t} \quad \textrm{if} \quad t \in (t^{0} - \delta)\\
  {0} \quad \textrm{otherwise}
\end{array}
```
```math
\textrm{NOTE: } {\delta} \textrm{ is the memory parameter.}
```
```math
\textrm{It represents the number of intervals after } {t^{0}} \textrm{ in which the customer can intervene with a rebounce effect.}
```
```math
\textrm{Where: } \quad {P^d_{i,t}, Q^d_{i,t}, SoC_{w,t}, P_{z,t}, Q_{z,t}, P^{cut}_{z,t}, P^{rb}_{z,t} \geq 0}
```
```math
\quad {P^g_{i,t}, Q^g_{i,t}, P_{f,t} \leq 0}
```
```math
\quad {- \infty \leq P_{w,t}, Q_{w,t}, Q_{f,t}, \Delta P_{i,t}, \Delta Q_{i,t}, P^{bus}_{i,t}, Q^{bus}_{i,t} \leq \infty}
```
<p align="right"><a href="#top">🔼 Back to top</a></p>

<!-- Storage System Model -->
### The Storage System Model
```math
\max_{P^{ess}_{t}, P^{bus}_{t}, SoC_{t}} \quad { \sum_{t=1}^{T} (\Pi^p_{t} \cdot P^d_{t} + \Pi^s_{t} \cdot P^i_{t}) }
```
```math
\textrm{s.t.} \quad {P^{bus}_{t} = P^d_{t} + P^i_{t}} \quad \forall t \in T
```
```math
\quad {P^{bus}_{t} = P^{load}_{t} + P^{gen}_{t} + P^{ess}_{t}} \quad \forall t \in T
```
```math
\quad \Biggl\{
\begin{array}{l}
  {SoC_{t} = SoC_{t-1} + \eta_{trip} \cdot P^{ess}_{t} \cdot \Delta t} \quad \textrm{if} \quad t \neq 1\\
  {SoC_{t} = SoC_{0} + \eta_{trip} \cdot P^{ess}_{t} \cdot \Delta t} \quad \textrm{if} \quad t = 1
\end{array} \quad \forall t \in T
```
```math
\quad {SoC^{min}_{t} \leq SoC_{t} \leq SoC^{max}_{t}} \quad \forall t \in T
```
```math
\quad {SoC_{0} = SoC_{T}}
```
```math
\quad {-P_{n} \leq P^{ess}_{t} - P^{ess}_{t-1} \leq P_{n}} \quad \forall t \in T
```
```math
\textrm{Where: } \quad {P^d_{i,t}, SoC_{t} \geq 0; \quad P^i_{j,t} \leq 0; \quad \forall t \in T}
```
<p align="right"><a href="#top">🔼 Back to top</a></p>

<!-- Auction Model -->
### The Auction Model of the Complex Bids Market
```math
\max_{P^d_{i,t},P^g_{j,t}, X_{k}} \quad { \sum_{t=1}^{T} ({\sum_{i=1}^{N_d} \Pi^d_{i,t} \cdot P^d_{i,t}} + {\sum_{j=1}^{N_g} \Pi^g_{j,t} \cdot P^g_{g,t}}) }
```
```math
\textrm{s.t.} \quad {\sum_{i=1}^{N_d} P^d_{i,t}} + {\sum_{j=1}^{N_g} P^g_{g,t}} = 0 \quad \forall t \in T
```
```math
\quad {X_i \cdot MAR \cdot {P^d_{i,t}}^{bid}} \leq P^d_{i,t} \leq {X_i \cdot {P^d_{i,t}}^{bid}} \quad \forall i \in N_d \quad \forall t \in T
```
```math
\quad {X_j \cdot {P^g_{j,t}}^{bid}} \leq P^g_{j,t} \leq {X_j \cdot MAR \cdot {P^g_{j,t}}^{bid}} \quad \forall j \in N_g \quad \forall t \in T
```
```math
\quad {\sum_{f=1}^{N^b_{flexi}} X^b_f} \leq 1 \quad \forall f \in N_{flexi-block}
```
```math
\quad {X^{child}_l} \leq {X^{parent}_l} \quad \forall l \in N_{child-parent-relationship}
```
```math
\quad {X_l \cdot MAR \cdot {P^d_{i,t}}^{bid}} \leq P^d_{i,t} \leq {X_l \cdot {P^d_{i,t}}^{bid}} \quad \forall i \in N_d \quad \forall l \in N_{child-parent-relationship} \quad \forall t \in T
```
```math
\quad {X_l \cdot {P^g_{j,t}}^{bid}} \leq P^g_{j,t} \leq {X_l \cdot MAR \cdot {P^g_{j,t}}^{bid}} \quad \forall j \in N_g \quad \forall l \in N_{child-parent-relationship} \quad \forall t \in T
```
```math
\textrm{Where: } \quad {P^d_{i,t} \geq 0 \quad  \forall i \in N_d; \quad P^g_{j,t} \leq 0 \quad \forall j \in N_g; \quad X_k \in \{0; 1\} \quad \forall k \in N_{child-parent-relationship}, N_{flexi-block}}
```
```math
\quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \quad \forall t \in T
```
<p align="right"><a href="#top">🔼 Back to top</a></p>



<!-- ROADMAP -->
## Roadmap

- [X] DLMP Implementation
- [X] Auction-based with complex bids
    - [X] Flexi & Linked orders for Storage
    - [ ] Complex orders for Generators
    - [X] Complex orders for Demand Response
- [X] Complex bids review
    - [X] Ramping
    - [X] Indivisibility
    - [X] Loop blocks
    - [X] Iceberg order
    - [X] Linked block orders
    - [X] Flexi block orders
    - [X] User-defined block orders
- [X] Complex bids implementation
    - [X] Python Version
- [ ] Continuous trading with simple bids
- [ ] Build scenario
    - [ ] Network
    - [ ] Prices
    - [ ] Resources & Profiles
    - [ ] Agent's behaviour
- [ ] Make assessment
- [ ] Analyse the results
- [ ] Write the paper
- [ ] Intelligent agents

<p align="right"><a href="#top">🔼 Back to top</a></p>



<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
<!-- To create your personalise shield go to: https://shields.io/ -->
[contributors-shield1]: https://img.shields.io/badge/Contributors-Matteo%20Troncia-orange
[contributors-url1]: https://www.iit.comillas.edu/people/mtroncia
[contributors-shield2]: https://img.shields.io/badge/Contributors-Marco%20Galici-green
[contributors-url2]: https://www.researchgate.net/profile/Marco-Galici
[contributors-shield3]: https://img.shields.io/badge/Contributors-Jose_Pablo_Chaves_Avila-skyblue
[contributors-url3]: https://www.iit.comillas.edu/personas/jchaves
[license-shield]: https://img.shields.io/badge/License-BSD_3_Clause-yellow
<!--https://img.shields.io/badge/License-MIT-yellow-->
[license-url]: https://github.com/MTr87/MKT_Comparison/blob/main/LICENSE
[linkedin-shield1]: https://img.shields.io/badge/LinkedIn-ID--Matteo%20Troncia-blue
[linkedin-url-1st]: https://es.linkedin.com/in/matteotroncia
[linkedin-shield2]: https://img.shields.io/badge/LinkedIn-ID--Marco%20Galici-lightgrey
[linkedin-url-2nd]: https://it.linkedin.com/in/marco-galici-493069190
[linkedin-shield3]: https://img.shields.io/badge/LinkedIn-ID--Jose_Pablo_Chaves_Avila-springgreen
[linkedin-url-3rd]: https://www.linkedin.com/in/jose-pablo-chaves-avila-06774084
[product-screenshot]: images/screenshot.png
[Python-shield]: https://img.shields.io/badge/Python-py-green
[Python-url]: https://www.python.org/
[Gurobi-shield]: https://img.shields.io/badge/Gurobi-py-red
[Gurobi-url]: https://www.gurobi.com/
