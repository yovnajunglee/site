--- 
title: An investigation into the effectiveness of infection control strategies
excerpt: "We build a compartmental data to investigate the effectiveness of isolation, quarantine, vaccination and PPE during SARS."
tags: [epidemiology, compartmental model, infectious diseases, SARS]
comments: true
category: blog
---


<script type="text/javascript" async 
        src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>




The 2003 SARS outbreak, which lead to 8432 reported cases and 813 related deaths, brought a shock to the affected countries and health organisations. In the absence of any vaccine or treatment, several countries became increasingly vulnerable. Mainland China was the most impacted country with 348 deaths from 5329 probable SARS cases as illustrated below. 

|<img src="/Plots/SARS/Map_Deaths-1.png" width="100%" height="100%">|<img src="/Plots/SARS/Map_cases-1.png" width="100%" height="100%">|

Due to the lack of information, the virus rapidly spread causing fatalities, especially among the elderly people. Healthcare facilities were under pressure, and the lack of protective equipment and training, contributed further to the spread of the disease.  Therefore, immediate responses and infection control measures were implemented to halt the epidemic. Border control measures were issued to prevent international spread and within countries, quarantine measures were in place. As more epidemiological data on SARS was gathered along with quarantine and isolation measures, the disease was effectively brought under control. The global epidemic ended in July 2003 and no cases have been reported since 2004. A lesson learnt from the outbreak was the importance of strong and coordinated public health facilities and an effective disease surveillance system to track the cases. 



Infection control measures such as quarantine, isolation, hospital equipment and vaccination have commonly been used in managing infectious diseases. However, the effectiveness of such methods can depend on the dynamics and epidemiology of the disease. Therefore, in this project, we compare and analyse the effectiveness of different strategies based on SARS epidemiology. Due to the heterogeneous transmission dynamics and disease effects of SARS on different groups, we categorise the population into healthcare workers, general public and the elderly people (>60). We develop a compartmental model to account for the different sub-populations of susceptible, exposed, vaccinated, quarantined, infectious, isolated and recovered individuals within each group.

A description of the model is given below:

|<img src="/Plots/SARS/SARS_model-1.png" width="70%" height="100%">|


|<img src="/Plots/SARS/model_des1.png" width="100%" height="100%">|
|<img src="/Plots/SARS/model_des2.png" width="100%" height="100%">|



The following results were obtained:

1. An optimal contact tracing system where all of the infected individuals are quarantined within 2 days will reduce the number of deaths and cases by up to 50\%.
2. Given that at least 80% of infected individuals are identified within a contact tracing system, the rate at which the quarantine occurs marginally affects the epidemic curves provided that this is done within at most 20 days.
3. Even though quarantine measures are in place, a lack of training and equipment in hospitals can exponentially increase the number of cases among the total population.
4. A vaccination strategy has a greater the impact if the vaccination is available during the early stages of the epidemic. 
5. There exists a trade-off between minimising the number of cases and deaths when a limited number of doses is available (2% of population). By allocating a large proportion of the doses to the elderly people, the number of deaths decreases but the number of cases increases.




