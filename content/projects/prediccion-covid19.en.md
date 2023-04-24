---
title: "Predicting the probability of death from COVID-19 in Mexico during 2020"
date: 2022-09-24T13:00:16-05:00
draft: false
showTableOfContents: true
#categories: ["Test", "Tutorial"]
tags: ["Machine learning", "Gradient boosting", "catboost"]
#layout: "simple"
summary: "How high were the probabilities of dying from COVID-19 in Mexico during 2020? On the most severe days of the pandemic over 90%"
slug: "covid19-prediction"
---
{{< lead >}}How high were the probabilities of dying from COVID-19 in Mexico during 2020? On the most severe days of the pandemic over 90%{{< /lead >}}

## The Impact of COVID-19
The COVID-19 pandemic has had serious consequences throughout the world and Mexico has been no exception. In certain months, the number of deaths caused by this disease pushed the health system to the limit and the effects still persist in many of the country's hospitals. INEGI has [reported](https://www.inegi.org.mx/contenidos/saladeprensa/boletines/2021/EstSociodemo/DefuncionesRegistradas2020preliminar.pdf) that of the total deaths registered during 2020 (1,086,743), 18.4% of these (200,256) were due to COVID-19, being the first cause of death in the case of men. This can be seen in more detail by analyzing the database of deaths during 2020 available in the [page](http://www.dgis.salud.gob.mx/contenidos/basesdedatos/da_defunciones_gobmx.html) of the Ministry of Health.

To try to give context to the situation experienced in the country during that year, I generated a model from this database that could predict, based on age, gender, place of death, education, date of death, among other variables, what were the probabilities that the cause of death of individuals would have been registered as COVID-19.

<gradio-app src="https://neek05-defunciones2020.hf.space"></gradio-app>

This model was developed with a *gradient boosting* algorithm, using the [Catboost library](https://catboost.ai/), obtaining an *F1 Score* of .80 after making several adjustments. Taking into account the changes in COVID-19 mortality that have occurred in later years as a consequence of the vaccination efforts carried out worldwide, this is not a model that can be extrapolated to later years. In this sense, it is more an exercise to put into practice knowledge learned in *machine learning* than a predictive model, and even more detailed results can be found and analyzed if the database made [available](http://www.dgis.salud.gob.mx/contenidos/basesdedatos/da_defunciones_gobmx.html) by the Ministry of Health is analyzed in detail.

## Probability of death due to COVID-19

![Gráfico de defunciones por COVID-19 en 2020](../GraficaDefunciones2020.png)

What this model can help us with, is to have a better idea of what was the *likelihood* that a person, in the course of 2020, would have COVID-19 as a cause of death, taking into consideration the sociodemographic characteristics previously mentioned: gender, age, date of death, locality of death, among others. What we find is that the probability of this being the cause of death increases significantly, in concordance with the graph above, which shows that the first wave of COVID-19 in 2020. 

![Causa de Fallecimiento](../CausadeFallecimiento.png)

For example, the chance that a 68-year-old man, in Mexico City, in the municipality of Gustavo A. Madero, with incomplete primary education, on December 22, 2020, would have died as a result of COVID-19 is up to 92%, which coincides with the highest peak of deaths in 2020 shown in the graph above.

The result changes radically if we consider the case of a woman of the same age and in the same locality, but who died in the first half of April. In this case, the probability that the cause of death was COVID-19 decreases to 39%. Playing a little more with the conditions of death, very interesting patterns can be found in different regions of the country depending on the selected temporality and demographic conditions. 

<script
	type="module"
	src="https://gradio.s3-us-west-2.amazonaws.com/3.27.0/gradio.js"
></script>