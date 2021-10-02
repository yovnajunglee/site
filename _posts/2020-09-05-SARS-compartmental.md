---
layout: post
title: Investigation the effectiveness of infection control strategies.
excerpt: "We build a compartmental data to investigate the effectiveness of isolation, quarantine, vaccination and PPE during an epidemic using SARS epidemiological data in Hong Kong."
tags: [intro, R, matrix factorisation, clustering]
comments: true
category: blog
---


<script type="text/javascript" async 
        src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>



The 2003 SARS outbreak, which lead to 8432 reported cases and 813 related deaths, brought a shock to the affected countries and health organisations. In the absence of any vaccine or treatment, several countries became increasingly vulnerable. Due to the lack of information, the virus rapidly spread causing fatalities, especially among the elderly people. Healthcare facilities were under pressure, and the lack of protective equipment and training, contributed further to the spread of the disease.  Therefore, immediate responses and infection control measures were implemented to halt the epidemic. Border control measures were issued to prevent international spread and within countries, quarantine measures were in place. As more epidemiological data on SARS was gathered along with quarantine and isolation measures, the disease was effectively brought under control. The global epidemic ended in July 2003 and no cases have been reported since 2004. A lesson learnt from the outbreak was the importance of strong and coordinated public health facilities and an effective disease surveillance system to track the cases. 

Infection control measures such as quarantine, isolation, hospital equipment and vaccination have commonly been used in managing infectious diseases. However, the effectiveness of such methods can depend on the dynamics and epidemiology of the disease. Therefore, in this project, we compare and analyse the effectiveness of different strategies based on SARS epidemiology. Due to the heterogeneous transmission dynamics and disease effects of SARS on different groups, we categorise the population into healthcare workers, general public and the elderly people ($>60$). We develop a compartmental model to account for the different sub-populations of susceptible, exposed, vaccinated, quarantined, infectious, isolated and recovered individuals within each group. The following results were obtained:




