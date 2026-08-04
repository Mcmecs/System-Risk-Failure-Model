# System Risk Model: A Technical Case Study

> **Disclaimer:** The source code and datasets for this project are proprietary and confidential to a previous employer. This repository serves strictly as a technical whitepaper outlining the processes, challenges, and methodologies used to solve a complex computational problem.

## Table of Contents

* [📊 Executive Summary](#-executive-summary)
* [🔨 The Tech Stack](#-the-tech-stack)
* [📓 Project Approach](#-project-approach)
  * [1. Data Ingestion & Event Probability Mapping](#1-data-ingestion--event-probability-mapping)
  * [2. Stochastic Distribution Fitting & Model Selection](#2-stochastic-distribution-fitting--model-selection)
  * [3. Multivariate Dependency Modeling via Copulas](#3-multivariate-dependency-modeling-via-copulas)
  * [4. Monte Carlo Simulation Engine](#4-monte-carlo-simulation-engine)
  * [5. Risk Visualization & Executive Reporting](#5-risk-visualization--executive-reporting)
* [🚧 Challenges Overcome](#-challenges-overcome)
* [🚀 Impact & Results](#-impact--results)
* [⏩ Evolution: The Capacity Risk Pipeline](#-evolution-the-capacity-risk-pipeline)

## 📊 Executive Summary

This project features a Python-based Jupyter Notebook model that quantifies the probability of commercial shortfalls of a transmission pipeline system from delivering natural gas to market. The Demand-Capacity interference analysis framework was used to calculate the shortfall.  

The model evaluates the mathematical intersection between the probability density functions (PDFs) of market gas demand and the pipeline network system’s flow capability. When these two distributions overlap, an interference zone is created, representing the statistical probability that the applied market demand will exceed the system’s available capacity. In addition, the system capacity impact from concurrent unit failure events, factoring in the likelihood of each occurring, was included for a realistic scenario.   

The project entailed the following core phases: 
* Calculating the probability of concurrent unit failures.
* Fitting empirical market data to probabilistic distributions. 
* Fitting hydraulically simulated system capability data to distributions.
* Simulating the interference between the distributions while considering tail dependency.

The primary objective of this project was to quantify system shortfall risk over a specific duration (e.g., a season). This data was used to provide executive decision-makers with a nuanced, data-driven risk matrix to rationalize increased contract flow levels across a pipeline network.

## 🔨 The Tech Stack

* **Language:** Python
* **Data Processing & Numerical Math:** Pandas, NumPy
* **Scientific & Statistical Computing:** SciPy, OpenTURNS, `itertools`, `scipy.stats`
* **Data Visualization:** Matplotlib, Seaborn
* **Domain:** Probabilistic Risk Analysis, Stochastic Modeling, Monte Carlo Simulations

## 📓 Project Approach

The tool was developed using sequential Python scripts within Jupyter Notebooks. This interactive format was chosen specifically to facilitate continuous data validation and allow for the manual testing of assumptions with Subject Matter Experts (SMEs) at each stage of the analysis.

### 1. Data Ingestion & Event Probability Mapping
* Processed and cleaned multi-year historical compressor unit availability logs using **Pandas** to calculate binary equipment failure rates.
* To model concurrent risk, Python's **`itertools`** library was utilized to algorithmically generate multi-unit simultaneous failure combinations. Evaluating up to 3 concurrent outages across 30+ compressor units yielded 4,525 mutually exclusive outage events.
* **NumPy** array operations were then applied to map joint event probabilities directly to their corresponding system capacity severity impacts.

### 2. Stochastic Distribution Fitting & Model Selection
* Python scripts were developed to clean and fit continuous probability density functions (PDFs) to historical market demand datasets.
* Hydraulically simulated flowrate results, representing the base capacity curve of the pipeline system generated via **Synergi Software**, were fitted with their own independent probability distributions.
* Statistical goodness-of-fit testing was conducted using one of the methods: **Kolmogorov-Smirnov (KS) tests**, alongside **Akaike Information Criterion (AIC)** and **Bayesian Information Criterion (BIC)** in **SciPy** and **OpenTURNS**, to evaluate and select the optimal distributions.

### 3. Multivariate Dependency Modeling via Copulas
* To accurately capture tail-dependence without falsely assuming linear correlation or statistical independence, the simulation engine integrated **copulas** to mathematically model the multivariate dependency structure between fluctuating market demand and base capacity curves.

### 4. Monte Carlo Simulation Engine
* The fitted base capacity distribution, market demand distribution, and combinatorial event probabilities were ingested into a multi-stage **Monte Carlo simulation** script.
* During execution, calculated combinatorial outage probabilities dynamically deforms and translates the base capacity distribution curve based on outage severity impact.
* The engine executed thousands of simulation iterations to evaluate dynamic Demand-Capacity interference and quantify seasonal shortfall risk.

### 5. Risk Visualization & Executive Reporting
* Simulation outputs were synthesized using **Matplotlib** and **Seaborn** to generate risk distribution plots.
* These visualizations translated complex mathematical interference zones and tail-risk scenarios into intuitive, actionable charts for non-technical corporate stakeholders.

## 🚧 Challenges Overcome

* **Legacy Codebase Refactoring & Scope Expansion:** A major engineering challenge involved deciphering and refactoring a predecessor’s legacy codebase while significantly expanding the architectural scope from single-unit evaluations to complex, multi-unit concurrent failure modeling.
* **Data Granularity & SME Validation:** Real-world industrial availability logs presented severe data quality anomalies. The historical dataset lacked critical granularity regarding scheduled versus unscheduled maintenance, demand versus undemand operating hours, and historical repair types (which dictate whether equipment reliability and life expectancy were extended). Solving this required extensive programmatic data scrubbing in Pandas coupled with rigorous Subject Matter Expert (SME) validation to establish accurate equipment availability baselines.
* **Memory & Vectorization Optimization:** Processing combinatorial failure arrays required replacing standard iterative loops with vectorized NumPy data structures. This optimization prevented memory bottlenecks and system crashes during execution.

## 🚀 Impact & Results

Results from this project were presented to executive decision-makers, providing a new quantitative metric that was utilized to justify increasing contract levels. 
Additionally, this model served as a proof-of-concept for refining conventional deterministic "worst-case scenario" planning. Unlike legacy static methods, this probabilistic framework successfully quantified actionable risk metrics—such as expected shortfall days per season—that were previously unavailable.

## ⏩ Evolution: The Capacity Risk Pipeline

This Jupyter Notebook project served as a highly successful, interactive proof-of-concept for solving complex Demand-Capacity interference. 

To see how I have since translated these core probabilistic concepts into a fully scalable, object-oriented software architecture, please view my follow-up independent project: [[Link to Capacity Risk Pipeline Repo](https://github.com/Mcmecs/capacity-shortfall-model.git)]. Built entirely from the ground up, it demonstrates the evolution from analytical scripts to a production-ready application.

---
*If you are a recruiter or engineering manager interested in discussing the probabilistic modeling, systems architecture, or data pipelines used in this project, I would be happy to connect.*

📫 **Let's Connect:** [www.linkedin.com/in/mark-y-cheung] | [mark.hwarang82@gmail.com]
