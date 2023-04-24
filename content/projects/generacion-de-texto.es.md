---
title: "Proyecto de generación de texto en español"
date: 2022-08-28T23:10:24-05:00
draft: false
showTableOfContents: true
#categories: ["Test", "Tutorial"]
tags: ["Machine learning", "RNN", "LSTM", "Transformers", "BLOOM"]
#layout: "simple"
summary: "Sobre la trayectoria seguida para lograr generar texto en español."
---
{{< lead >}}Sobre la trayectoria seguida para lograr generar texto en español{{< /lead >}}

## Generación de texto en español
Quiero empezar este texto mostrando inmediatamente el resultado del primer proyecto que realicé en *machine learning*: un generador de texto en español a partir de hacer *fine-tuning* a un modelo existente (*BLOOM*) usando *Transformers*. El entrenamiento se realizó a partir de las transcripciones de las conferencias de prensa y discursos públicos del presidente de México, Andrés Manuel López Obrador, por lo que el texto generado se caracteriza por replicar su particular manera de comunicarse de forma verbal.

<gradio-app space="neek05/NLP-AMLO"></gradio-app> 

A partir de aquí el relato de la trayectoria para llegar a este resultado, **aclarando que el texto no pretende ser un tutorial ni explicar los detalles específicos para la creación de esta app.**

## Planteando el proyecto

En mi trayectoria por aprender *recurrent neural networks* (RNN) decidí que uno de los primeros proyectos que trabajaría de forma personal fuese uno de generación de texto a nivel de caracteres, lo cual me ayudaría a comprender mejor su funcionamiento y a poner en práctica lo aprendido. Decidí iniciar el proyecto trabajando a partir textos de Donald Trump, al considerar que habría mucho material disponible, si bien pronto se volvió evidente que no era [precisamente](https://github.com/ZaydH/trump_char_rnn) [la idea](https://www.csail.mit.edu/news/postdoc-develops-twitterbot-uses-ai-sound-donald-trump) [más](https://towardsdatascience.com/predicting-trump-tweets-with-a-rnn-95e7c398b18e) [novedosa](https://github.com/ppramesi/RoboTrumpDNN).

Para darle un poco más de originalidad y diversidad al asunto, elegí trabajar no a partir de textos escritos por el expresidente en libros o *tweets*, sino a partir de las transcripciones de sus discursos públicos, por lo que podríamos ver este ejercicio como un generador de discursos. 

## Primeros resultados y cambio de enfoque

Utilizando una red neuronal LSTM (*Long Short Term Memory*) en *PyTorch* y con un entrenamiento de 30 *epochs* los primeros resultados obtenidos fueron... curiosos: 


> The media is going to be talking about the farmers. They’re a very good job and we are trying to be a great job that they have to be and with the problem. I think it’s not allowed to get it, they don’t want to say it. They want to give them all the time. I think it will start to get along with a land. We had a great service and the first time to say the sand and energy independence, but when the suburbs, they’ve been able to do that...


Como ejercicio resultó francamente entretenido y el resultado divertido de leer. El texto generado, si bien se puede apreciar el estilo de expresarse del expresidente estadounidense, carece de coherencia. Sin embargo en un modelo entrenado desde cero, con un hardware limitado, era esperable que los resultados no fueran espectaculares en el primer intento. Habiendo leído ya un poco de literatura de sobre *transfer learning* me pareció que este podría ser un excelente ejercicio para ponerlo en la práctica y ver los beneficios que podría obtener utilizando modelos previamente entrenados.

## Utilizando *transformers* y GPT-2

El siguiente paso en este proyecto fue documentarme sobre el funcionamiento de la librería *Transformers* de [*Hugging Face*](https://huggingface.co/) y tratar de mejorar los resultados previamente obtenidos. El modelo utilizado en esta ocasión fue GPT-2, modelo de generación de texto puesto a disposición por *OpenAI* en 2019. Después de mucha prueba y error y tras ir ajustando diversos parámetros en el modelo, logre generar los siguientes textos:

![Generación de texto con GPT-2](../ResultadosGPT-2.png)

Se observan resultados de mucha mayor calidad que en el primer ejercicio y en una cantidad de tiempo relativamente breve, lo cual indicaba que este era un buen camino por seguir.

## Modelos para generar texto en español

Sin embargo, lo que realmente deseaba realizar desde un primer momento era la generación de texto en español, tratando de replicar la calidad de los resultados obtenidos entrenando el modelo en inglés. De ahí que consideré que una vez obtenidos resultados aceptables en la generación de texto en inglés, sería sencillo replicar los resultados con texto en español. Desafortunadamente, esto no fue así y ya ciertos [artículos](https://www.vanderbilt.edu/digitalhumanities/gpt-2-no-habla-espanol-artificial-intelligence-anglocentrism-and-the-non-human-side-of-dh/) mencionan las limitaciones del modelo GPT-2 para la generación de texto en idiomas distintos al inglés.  

Si bien se encuentran disponibles en [*Hugging Face*](https://huggingface.co/models) modelos de GPT-2 entrenados con un corpus en español, tras haber leido recientemente sobre esta iniciativa, decidí que sería un buen ejercicio probar el modelo multilenguaje de [*BLOOM*](https://huggingface.co/bigscience/bloom), proyecto de ciencia abierta y de acceso libre entrenado para generar texto en 46 idiomas y que fue hecho público en julio de este mismo año.

## Web scraping

En esta ocasión decidí que la generación de texto en español sería a partir de transcripciones conferencias y discursos del presidente Andrés Manuel López Obrador, y el motivo por el cual seleccionar a este personaje fue similar al anterior: amplia disponibilidad de material en internet. El proceso para obtener las transcripciones partió de hacer *web scraping* desde la página del [presidente](https://lopezobrador.org.mx/transcripciones/) donde se encuentran disponibles estas transcripciones.

Utilizando [Scrapy](https://scrapy.org/) obtuve 1609 transcripciones de conferencias de prensa y discursos en eventos públicos del 4 de diciembre de 2018 al 21 de julio de 2022. Una vez realizada la limpieza de las transcripciones y manteniendo únicamente las participaciones del presidente López Obrador y eliminando las participaciones de cualquier otro personaje, esto se tradujo en 38 MB de discursos y conferencias transcritas, equivalente a 36,795,294 caracteres.

## Entrenamiento del modelo y conclusiones
Para poder entrenar el modelo y debido a las limitaciones de hardware tuve que hacer varios ajustes: en primer lugar utilizar el modelo de BLOOM más pequeño disponible (que aun así cuenta con 560 millones de parámetros), mantener únicamente el 25% del texto obtenido, que a su vez, para evitar problemas con la RAM, tuve que dividir en oraciones completas con un máximo de 1000 caracteres. 

Otra limitación adicional, fue que al momento de programar la aplicación utilizando *Gradio* y al subirla a [*Hugging Spaces*](https://huggingface.co/spaces), por cuestión del tiempo requerido para generar el texto, vi necesario limitar la generación a solo 100 palabras.

El resultado del *fine tuning* tras un entrenamiento de 5 *epochs*, es la generación de un texto decente, como se puede ver al inicio, que a diferencia de los primeros ejercicios, mantiene al menos una coherencia y estructura dentro de las oraciones generadas. Considero que aún hay margen de mejora y afortunadamente la *API* de *transformers* tiene la suficiente flexibilidad para ir ajustando distintos parámetros tanto en el entrenamiento como en la generación de texto. Espero subir próximamente un tutorial detallado sobre los pasos seguidos en este proyecto.

<script type="module"
src="https://gradio.s3-us-west-2.amazonaws.com/3.27/gradio.js">
</script>