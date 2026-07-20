# Probabilistic Risk Assessment Tool: A Technical Case Study

> **Disclaimer:** The source code, specific algorithms, and datasets for this project are proprietary and confidential to a previous employer. This repository serves strictly as a technical whitepaper outlining the architecture, challenges, and methodologies used to solve a complex computational problem.

## 📊 Executive Summary
This project features a Probabilistic Risk Assessment (PRA) tool that calculates the probability of system shortfall by analyzing Demand-Capacity interference. The model evaluates the mathematical intersection between the probability density functions (PDFs) of market gas demand and the pipeline network system’s flow capability. When these two distributions overlap, an interference zone is created, representing the statistical probability that the applied market demand will exceed the system’s available capacity.

In a large industrial network (such as a major natural gas pipeline or a multi-node data center), the isolated failure of a major equipment unit, like a compressor or a server rack, will have minimal impact on overall system capacity if the network is engineered with sufficient backup redundancy. However, the concurrent failure of multiple independent units with specific system quirks or unevenly distributed capability, combined with peak market demand conditions, can lead to cascading operational bottlenecks and severe commercial shortfalls. 

The primary objective of this project was to quantify system shortfall risk, specifically the probability of deficit and expected shortfall days per season, by modeling how near-future market demand distributions overlap with system capacity curves while accounting for random, multi-unit equipment failures. By replacing static, deterministic "worst-case scenario" assumptions with dynamic stochastic modeling, the tool provided executive decision-makers with a nuanced, data-driven risk matrix to rationalize increased contract flow levels across a major pipeline network.

## 🛠️ The Tech Stack
* **Language:** Python
* **Data Processing & Numerical Math:** Pandas, NumPy
* **Scientific & Statistical Computing:** SciPy, OpenTURNS, `itertools`, `scipy.stats`
* **Data Visualization:** Matplotlib, Seaborn
* **Domain:** Probabilistic Risk Assessment (PRA), Combinatorial Optimization, Stochastic Modeling, Monte Carlo Simulations

## ⚙️ System Architecture & Approach
Rather than a monolithic enterprise application, this tool was architected as a modular analytical pipeline composed of sequential Python scripts that processed enterprise data from initial ingestion through to stochastic simulation.

### 1. Data Ingestion & Event Probability Mapping
* The pipeline processed and cleaned multi-year historical compressor unit availability logs using **Pandas** to calculate binary equipment failure rates.
* To model concurrent risk, Python's **`itertools`** library was utilized to algorithmically generate multi-unit simultaneous failure combinations. Evaluating up to 3 simultaneous outages across 30+ compressor units yielded an active sample space of 4,525 mutually exclusive outage events ($\binom{30}{1} + \binom{30}{2} + \binom{30}{3}$).
* **NumPy** array operations were then applied to map joint event probabilities directly to their corresponding system capacity severity impacts.

### 2. Stochastic Distribution Fitting & Model Selection
* Python scripts were developed to clean and fit continuous probability density functions (PDFs) to historical market demand datasets.
* Hydraulically simulated flowrate results, representing the base capacity curve of the pipeline system generated via **Synergi Software**, were fitted with their own independent probability distributions.
* Rigorous statistical goodness-of-fit testing was conducted using **Kolmogorov-Smirnov (KS) tests**, alongside **Akaike Information Criterion (AIC)** and **Bayesian Information Criterion (BIC)** in **SciPy** and **OpenTURNS**, to evaluate and select the optimal distributions.

### 3. Multivariate Dependency Modeling via Copulas
* In real-world industrial networks, market demand and system capacity often exhibit complex, non-linear dependencies (e.g., peak market demand in specific regions correlating with high system strain, or inverse relationships during seasonal transitions).
* To accurately capture tail-dependence without falsely assuming linear correlation or statistical independence, the simulation engine integrated **copulas** to mathematically model the multivariate dependency structure between fluctuating market demand and base capacity curves.

### 4. Monte Carlo Simulation Engine
* The fitted base capacity distribution, market demand distribution, and combinatorial event probabilities were ingested into a multi-stage **Monte Carlo simulation** script.
* During execution, calculated combinatorial outage probabilities dynamically shifted the base capacity distribution curve based on outage severity impact.
* The engine executed thousands of simulation iterations to evaluate dynamic Demand-Capacity interference and quantify seasonal shortfall risk.

### 5. Risk Visualization & Executive Reporting
* Simulation outputs were synthesized using **Matplotlib** and **Seaborn** to generate comprehensive statistical dashboards and risk distribution plots.
* These visualizations translated complex mathematical interference zones and tail-risk scenarios into intuitive, actionable charts for non-technical corporate stakeholders.

## 🚧 Challenges Overcome
* **Legacy Codebase Refactoring & Scope Expansion:** A major engineering challenge involved deciphering and refactoring a predecessor’s legacy codebase while significantly expanding the architectural scope from single-unit evaluations to complex, multi-unit concurrent failure modeling.
* **Data Granularity & SME Validation:** Real-world industrial availability logs presented severe data quality anomalies. The historical dataset lacked critical granularity regarding scheduled versus unscheduled maintenance, demand versus undemand operating hours, and historical repair types (which dictate whether equipment reliability and life expectancy were extended). Solving this required extensive programmatic data scrubbing in Pandas coupled with rigorous Subject Matter Expert (SME) validation to establish accurate equipment availability baselines.
* **Memory & Vectorization Optimization:** Processing combinatorial failure arrays required replacing standard iterative loops with vectorized NumPy data structures. This optimization prevented memory bottlenecks and system crashes during execution.

## 🚀 Impact & Results
* **Reliability KPIs:** By translating abstract statistical mathematics into intuitive commercial metrics, specifically establishing the *expected shortfall days per season*, the tool bridged the gap between engineering risk and customer communication. This metric established a measurable reliability baseline, enabling commercial teams to structure new contract packages and evaluate cost-risk trade-offs (e.g., modeling the exact operational or maintenance expenditure required to achieve a targeted X% improvement in system reliability). 
* **Eliminating Design Conservatism & CapEx Waste:** Traditional deterministic engineering models rely on static, "worst-case scenario" assumptions. This historical approach created unquantified layers of conservatism that often drove capital-intensive decisions to build redundant facilities. By modeling the tail-risk of concurrent multi-unit outages under fluctuating demand, the probabilistic tool indicated that the network could safely absorb higher capacities, avoiding massive unnecessary capital expenditure (CapEx) required by legacy deterministic rules. 


## 🗺️ Future Architectural Roadmap
As a self-taught project developed to solve an immediate industrial need, the pipeline successfully delivered actionable executive insights. However, viewing the architecture through the lens of formal software and systems engineering highlights several key areas for future technical evolution:
* **Object-Oriented Programming (OOP) Refactoring:** Transitioning the current modular script architecture into a unified, Object-Oriented software design. Encapsulating compressor units, distribution fitters, and simulation engines into distinct classes would significantly improve code cleanliness, maintainability, and extensibility.
* **Automated Data Pipelines & Orchestration:** Automating manual hand-offs between the data cleaning, distribution fitting, and Monte Carlo simulation stages. Implementing data orchestration workflows (such as Apache Airflow or CI/CD pipelines) would transform the tool from a standalone script suite into an automated enterprise service.
* **Computational Performance Optimization:** The current Monte Carlo simulation engine requires approximately 2 hours to execute full-scale simulation runs. Future iterations would explore multiprocessing, parallel computing, or migrating core mathematical calculations to optimized C++/Pybind11 backends to dramatically reduce computational runtime.

---
*If you are a recruiter or engineering manager interested in discussing the probabilistic modeling, systems architecture, or data pipelines used in this project, I would be happy to connect.*

📫 **Let's Connect:** [www.linkedin.com/in/mark-y-cheung] | [mark.hwarang82@gmail.com]
