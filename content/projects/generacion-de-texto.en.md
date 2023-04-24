---
title: "Spanish text generation project"
date: 2022-08-28T23:10:24-05:00
draft: false
showTableOfContents: true
#categories: ["Test", "Tutorial"]
tags: ["Machine learning", "RNN", "LSTM", "Transformers", "BLOOM"]
#layout: "simple"
summary: "On the path followed to generate text in Spanish"
slug: "text-generation"
---
{{< lead >}}On the path followed to generate text in Spanish{{< /lead >}}

## Spanish text generation
I want to start this text by immediately showing the result of the first project I did in *machine learning*: a Spanish text generator from *fine-tuning* an existing model (*BLOOM*) using *Transformers*. The training was carried out from the transcripts of the press conferences and public speeches of the president of Mexico, Andrés Manuel López Obrador, so the generated text is characterized by replicating his particular way of communicating verbally.

<gradio-app src="https://neek05-nlp-amlo.hf.space"></gradio-app>

From here on, the story of the path to reach this result, **clarifying that the text is not intended to be a tutorial or to explain the specific details for the creation of this app.**

### Setting up the project

In my journey to learn *recurrent neural networks* (RNN) I decided that one of the first projects I would work on personally would be one of character-level text generation, which would help me to better understand how it works and to put into practice what I had learned. I decided to start the project by working from Donald Trump's texts, considering that there would be a lot of material available, although it soon became evident that it was not [exactly](https://github.com/ZaydH/trump_char_rnn) [the most](https://www.csail.mit.edu/news/postdoc-develops-twitterbot-uses-ai-sound-donald-trump) [innovative](https://towardsdatascience.com/predicting-trump-tweets-with-a-rnn-95e7c398b18e) [idea](https://github.com/ppramesi/RoboTrumpDNN).

To give a little more originality and diversity to the subject, I chose to work not from texts written by the former president in books or *tweets*, but from the transcripts of his public speeches, so we could see this exercise as a speech generator. 

## First results and change of approach

Using a LSTM (*Long Short Term Memory*) neural network in *PyTorch* and with a training of 30 *epochs* the first results obtained were... curious: 


> The media is going to be talking about the farmers. They’re a very good job and we are trying to be a great job that they have to be and with the problem. I think it’s not allowed to get it, they don’t want to say it. They want to give them all the time. I think it will start to get along with a land. We had a great service and the first time to say the sand and energy independence, but when the suburbs, they’ve been able to do that...


As an exercise it was frankly entertaining and the result was fun to read. The generated text, although the style of expression of the former US president can be appreciated, lacks coherence. However, in a model trained from scratch, with limited hardware, it was to be expected that the results wouldn't be spectacular on the first try. Having already read some literature on *transfer learning* it seemed to me that this could be an excellent exercise to put into practice and see the benefits that could be obtained using previously trained models.

## Using *transformers* and GPT-2

The next step in this project was to document myself on the operation of the *Transformers* library from [*Hugging Face*](https://huggingface.co/) and try to improve the results previously obtained. The model used this time was GPT-2, a text generation model made available by *OpenAI* in 2019. After much trial and error and after adjusting several parameters in the model, I managed to generate the following texts:

![Generación de texto con GPT-2](../ResultadosGPT-2.png)

Much higher quality results were observed than in the first exercise and in a relatively short amount of time, which indicated that this was a good way to go.

## Models to generate text in Spanish

However, what I really wanted to do from the beginning was to generate text in Spanish, trying to replicate the quality of the results obtained by training the model in English. Hence, I considered that once acceptable results were obtained in the generation of English text, it would be easy to replicate the results with Spanish text. Unfortunately, this was not the case and some [articles](https://www.vanderbilt.edu/digitalhumanities/gpt-2-no-habla-espanol-artificial-intelligence-anglocentrism-and-the-non-human-side-of-dh/) already mention the limitations of the GPT-2 model for text generation in languages other than English.  

While models of GPT-2 trained with a Spanish corpus are available at [*Hugging Face](https://huggingface.co/models), having recently read about this initiative, I decided it would be a good exercise to test the multilingual model of [*BLOOM](https://huggingface.co/bigscience/bloom), an open science and open access project trained to generate text in 46 languages and made public in July of this year.

## Web scraping

This time I decided that the generation of text in Spanish would be from transcriptions of conferences and speeches of President Andrés Manuel López Obrador, and the reason for selecting this character was similar to the previous one: wide availability of material in the internet. The process to obtain the transcripts was based on *web scraping* from the [president's] web page (https://lopezobrador.org.mx/transcripciones/) where these transcripts are available.

Using [Scrapy](https://scrapy.org/) I obtained 1609 transcripts of press conferences and speeches at public events from December 4, 2018 to July 21, 2022. After cleaning the transcripts and keeping only the participations of President López Obrador and eliminating the interventions of any other character, this resulted in 38 MB of transcribed speeches and conferences, equivalent to 36,795,294 characters.

## Model training and conclusions
In order to train the model and due to hardware limitations I had to make several adjustments: firstly use the smallest BLOOM model available (which still has 560 million parameters), keep only 25% of the text obtained, which in turn, to avoid problems with RAM, I had to divide into complete sentences with a maximum of 1000 characters. 

Another additional limitation was that when programming the application using *Gradio* and uploading it to [*Hugging Spaces*](https://huggingface.co/spaces), due to the time required to generate the text, I found it necessary to limit the generation to only 100 words.

The result of the *fine tuning* after a training of 5 *epochs*, is the generation of a decent text, as you can see at the beginning, which unlike the first exercises, maintains at least a coherence and structure within the generated sentences. I consider that there's still room for improvement and fortunately the *transformers* *API* has enough flexibility to adjust different parameters both in the training and in the text generation. I hope to upload soon a detailed tutorial about the steps followed in this project.

<script
	type="module"
	src="https://gradio.s3-us-west-2.amazonaws.com/3.27/gradio.js"
></script>
