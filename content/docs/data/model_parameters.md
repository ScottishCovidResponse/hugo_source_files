---
title: Model parameters
weight: 30
---

| **Parameter** | **Source** | **LSHTM** | **Simulation.jl** |
| ------------- | ---------- | --------- | ----------------- |
| Latent period | <a href="#ref2">2</a>, <a href="#ref3">3</a>, <a href="#ref4">4</a> | {{< katex display >}} \footnotesize{d_E: \Gamma(\mu=4.0,k=4)} {{< /katex >}} | - |
| Pre-Clinical Infectiousness Duration | <a href="#ref5">5</a> | {{< katex display >}} \footnotesize{d_P: \Gamma(\mu=1.5,k=4)} {{< /katex >}} | - |
| Clinical Infectiousness Duration | <a href="#ref2">2</a>, <a href="#ref3">3</a>, <a href="#ref4">4</a> |  {{< katex display >}} \footnotesize{d_C: \Gamma(\mu=3.5,k=4)} {{< /katex >}} | - |
| Subclinical Infectiousness Duration <br> <sub>*Assumed to be the same duration as total infectious period for clinical cases, including preclinical transmission*</sub> | <a href="#ref1">1</a> | {{< katex display >}} \footnotesize{d_S: \Gamma(\mu=5.0,k=4)} {{< /katex >}} | - |
| Hospitalization | - | {{< katex display >}} \footnotesize{d_H: 1} {{< /katex >}} | - |
| Incubation period | <a href="#ref1">1</a> | {{< katex display >}} \footnotesize{d_E+d_P; \mu=5.5} {{< /katex >}} | - |
| Relative infectiousness of subclinical cases | <a href="#ref1">1</a> | {{< katex display >}} \footnotesize{f = 50\%} {{< /katex >}} | - |
| UK Contact Matrices <br> <sub>*Number of age-{{< katex >}}j{{< /katex >}} individuals contacted by age-{{< katex >}}i{{< /katex >}} individual per day*</sub> | <a href="#ref6">6</a> | {{< katex display >}} \footnotesize{c_{ij}:} {{< /katex >}} `covidm/data/all_matrices.rds` | - |
| Number of age-{{< katex >}}i{{< /katex >}} individuals | <a href="#ref6">6</a> | {{< katex display >}} \footnotesize{N_i} {{< /katex >}} | - |
| Proportion of hospitalised cases requiring critical care | <a href="#ref7">7</a> |{{< katex display >}} \footnotesize{30\%} {{< /katex >}} | - |
| Serial Interval | <a href="#ref2">2</a>, <a href="#ref3">3</a>, <a href="#ref4">4</a> | {{< katex display >}} \footnotesize{d_E + \tfrac{1}{2}(y_i(d_P+d_C)+(1-y_i)d_S) = 6.5} \text{ days} {{< /katex >}} | - |
| Delay from onset to hospitalization | <a href="#ref7">7</a>, <a href="#ref8">8</a> | {{< katex display >}} \footnotesize{\Gamma(\mu=7,k=7)} {{< /katex >}} | - |
| Duration of hospitalization | <a href="#ref7">7</a> | {{< katex display >}} \footnotesize{\Gamma(\mu=10,k=10)} {{< /katex >}} | - |
| Proportion of hospitalized cases requiring critical care | <a href="#ref7">7</a> | {{< katex display >}} \footnotesize{30\%} {{< /katex >}} | - |
| Delay from onset to death | <a href="#ref7">7</a>, <a href="#ref8">8</a> |  {{< katex display >}} \footnotesize{\Gamma(\mu=22,k=22)} {{< /katex >}} | - |
| | | {{< katex display >}} \footnotesize{dIp} {{< /katex >}} | - |

<a name="ref1"></a>1 [The effect of non-pharmaceutical interventions on COVID-19 cases, deaths and demand for hospital services in the UK: a modelling study, N. G. Davies et. al, 2020, Table S1, pg. 23.](https://data.scrc.uk/object/23270)

<a name="ref2"></a>2: [Early Transmission Dynamics in Wuhan China, of Novel Coronavirus-Infected Pneumonia, L. Q. Guan et. al., 2020, N Engl J Med. 2020;382: 1199-1207.](https://data.scrc.uk/object/24857)

<a name="ref3"></a>3: Epidemiology and Transmission of COVID-19 in Shenzhen China: Analysis of 391 cases and 1,286 of their close contacts, medRxiv. 2020;2020.03.03.20028423.

<a name="ref4"></a>4: Serial interval of novel coronavirus (2019-nCoV) infections, medRxiv. 2020; 2020.02.03.20019497

<a name="ref5"></a>5: [The contribution of pre-symptomatic infection to the transmission dynamics of COVID-2019, Liu Y et. al, Wellcome Open Research. 2020;5:58.](https://data.scrc.uk/object/40955)

<a name="ref6"></a>6: Social contacts and mixing patterns relevant to the spread of infectious diseases, J. Mossong et. al, PLoS Med. 2008;5 e74.

<a name="ref7"></a>7: [A Trial of Lopinavir-Ritonavir in Adults Hospitalized with Sever Covid-19, B. Cao et. al, N Engl H Med. 2020. doi:10.1056/NEJMoa2001282.](https://data.scrc.uk/object/24871)

<a name="ref8"></a>8: [Incubation Period and Other Epidemiological Characteristics of 2019 Novel Coronavirus Infections with Right Truncation: A Statistical Analysis Of Publicly Available Case Data, N. M. Linton et. al, J Clin Med Res. 2020;9. doi:10.3390/jcm9020538.](https://data.scrc.uk/object/17254)