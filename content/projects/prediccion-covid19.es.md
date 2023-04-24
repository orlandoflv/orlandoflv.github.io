---
title: "Prediciendo la probabilidad de muerte por COVID-19 en México durante 2020"
date: 2022-09-24T13:00:16-05:00
draft: false
showTableOfContents: true
#categories: ["Test", "Tutorial"]
tags: ["Machine learning", "Gradient boosting", "catboost"]
#layout: "simple"
summary: "¿Qué tan altas fueron las probabilidades de morir por COVID-19 en México durante 2020? En los días más graves de la pandemia superiores al 90%"
---
{{< lead >}}¿Qué tan altas fueron las probabilidades de morir por COVID-19 en México durante 2020? En los días más graves de la pandemia superiores al 90%{{< /lead >}}

## Impacto de la COVID-19
La pandemia de COVID-19 ha tenido consecuencias graves en todo el mundo y en México no ha sido la excepción. En determinados meses, la cantidad de muertes ocasionadas por esta enfermedad puso al límite al sistema de salud y aún persisten los efectos en muchos de los hospitales del país. El INEGI ha [reportado](https://www.inegi.org.mx/contenidos/saladeprensa/boletines/2021/EstSociodemo/DefuncionesRegistradas2020preliminar.pdf) que del total de muertes registradas durante el 2020 (1,086,743), 18.4% de estas (200,256) fueron debido a la COVID-19, siendo la primer causa de muerte en el caso de los hombres. Lo anterior se puede observar más detalladamente analizando la base de datos de defunciones durante el 2020 disponible en la [página](http://www.dgis.salud.gob.mx/contenidos/basesdedatos/da_defunciones_gobmx.html) de la Secretaria de Salud del Gobierno Federal. 

Para tratar de dar contexto a la situación que se vivió en el país durante ese año generé un modelo, a partir de esta base de datos, que pudiese predecir, en función de la edad, género, lugar de fallecimiento, escolaridad, fecha de fallecimiento, entre otras variables, cuáles fueron las probabilidades de que la causa de defunción de las personas se hubiese registrado como COVID-19.

<gradio-app space="neek05/Defunciones2020"></gradio-app> 

Este modelo fue desarrollado con un algoritmo de *gradient boosting*, utilizando la librería de [Catboost](https://catboost.ai/), logrando obtener un *F1 Score* de .80 después de realizar varios ajustes. Tomando en cuenta los cambios en la mortalidad por COVID-19 que se han presentado en años posteriores como consecuencia de los esfuerzos de vacunación llevados a cabo a nivel mundial, no es un modelo que pueda extrapolarse a años posteriores. En este sentido es más un ejercicio para poner en práctica conocimientos aprendidos en *machine learning* que un modelo predictivo e incluso, si se analiza detalladamente la base de datos puesta a [disposición](http://www.dgis.salud.gob.mx/contenidos/basesdedatos/da_defunciones_gobmx.html) por la Secretaria de Salud se pueden encontrar y analizar resultados más detallados.

## Probabilidad de fallecimiento por COVID-19

![Gráfico de defunciones por COVID-19 en 2020](../GraficaDefunciones2020.png)

En lo que nos puede ayudar este modelo es tener una mejor idea de cuál fue la *probabilidad* de que una persona, en el transcurso de 2020, tuviese como causa de muerte la COVID-19, tomando en consideración las características sociodemográficas previamente mencionadas: género, edad, fecha de fallecimiento, localidad de fallecimiento, ser derechohabiente, entre otras. Lo que encontramos es que la probabilidad de que sea esta la causa de muerte aumenta significativamente, en concordancia con el gráfico superior que muestra que la primera ola de COVID-19 en 2020. 

![Causa de Fallecimiento](../CausadeFallecimiento.png)

Por ejemplo, la posibilidad de que un hombre de 68 años, en la Ciudad de México, en el municipio de Gustavo A. Madero, con Primaria incompleta, no derechohabiente, el 22 de diciembre de 2020, haya fallecido como consecuencia de la COVID-19 es de hasta el 92%, lo cual coincide con el pico más altos de fallecimientos en 2020 que se muestra el gráfico de arriba.

El resultado cambia radicalmente si consideramos el caso de una mujer de esa misma edad y en esa misma localidad, pero fallecida en la primera quincena de abril. En este caso la probabilidad de que la causa de fallecimiento haya sido COVID-19 disminuye a 39%. Jugando un poco más con las  condiciones de deceso, se podrán encontrar patrones muy interesantes en distintas regiones del país en función de la temporalidad seleccionada  y las condiciones demográficas. 

<script type="module"
src="https://gradio.s3-us-west-2.amazonaws.com/3.27/gradio.js">
</script>