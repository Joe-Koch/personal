---
layout: single
title: Reddit Generator
excerpt: "Teaching a machine to shitpost."
author_profile: true
sidebar:
  - text: "<a href='https://github.com/ZoeKoch/reddit-comment-generator/blob/master/generator.ipynb'>View the project's code</a>"
toc: true
tags:
  - neural network
  - text data
  - generation
  - Python
---

# Training a machine to generate reddit comments

## The Data

The model was trained on all reddit comments from May 2015, downloaded from [Kaggle](https://www.kaggle.com/reddit/reddit-comments-may-2015). Reddit is a global site, so some of the comments aren’t written in the Latin alphabet, and more aren’t written in English. For this assignment, I’m only using the parts of comments written in ASCII characters. The comments are then broken into 40-length character chunks, which are put into a one hot encoding for each character. Time/CPU constraints meant restricting this dataset further, to ~800 comments. Full use of the dataset would’ve yielded better results.

## The Model

The baseline architecture is an RNN, produced with Keras’s SimpleRNN built-in, with 128 layers, a bias, and ‘glorot_uniform’ initial weights, followed by a dense layer. The activation function was a softmax, and the optimizer was the Adam optimizer, with a learning rate of 0.001, and no decay. Loss was computed with categorical cross entropy. Training was done in batch sizes of 100.

Example comment after 100 epochs:

> . had wing tlike burspe buthas gheshite tea lonet mG tive pinis soperisupre of marwiend jest wurknt tadee shat at ofselly josld cetthey itwing/ oushonce jorg and if ke that them. N-_Ke?LI nge picpeabdernf whathithens ghe fery tee thare lorshesctine to thien afferemtenisher obvenat, ing thougking? I knoid fo(tke mponge. I faysh woutd ge dope ion Plot enostie so.. Shas'st cammonive ffyea conitt. ." 

{% include figure image_path="/assets/images/reddit/loss1.png" alt=""%}

## Further experiments

We see the loss with half the number of hidden units and twice the number of hidden units, respectively.

Half hidden units example:
> halsat you mone soles arey"r thever ale  I gnt think eas wathe betiselry ways, de bats. YTlingead a mare nosterohe pactuth then aly tut as mine tifcind/be oust laly or tllith4 houchne therescorany that. Then tome bom. Sueds. Therwitteatpangiy lpocher on wact lik. M I Swivt redienthat soulican's then afening and r oucl chey n hea teanet loke dore somalive out  hay donghise bo thipcrtbyine why uwost

{% include figure image_path="/assets/images/reddit/loss2.png" alt=""%}

Double hidden units example:
> She the,.o
>nt? sole_t0Ps poug.tent would acsid tond preomero Ive poont5 gorselacen uaven Anare a le sheKqu. Pho fo you gols onen's at     (feroraideo iI ctus) Georipr, I mave mLofis 1] st and _ling. Yt; me touebpores or thete aod to out tha wW scactoneld barangion d red inking go s but thay7el at on thlocreoblyoo at Yea lit?

> I jusle shat and the woon pre, Doncerfer a tivend I don, wxer the hM ?.Eoe. 

{% include figure image_path="/assets/images/reddit/loss3.png" alt=""%}

An LSTM model shows extremely similar results to the simple RNN in terms of loss function. Looking at the example text, I think it’s a bit better, with good apostrophe/comma use and some actual words in there.

Example text: 

> . Meotwe sh helly I covena do bay whet and it are difit chone to she renn ppactimacke a simand in readind wet cande snay. ann bithan a but your eanhe done ald reasny weve tfio so I waysh a y uisteis con't, twing a frempristmenarcald? I mintt bacle st ree pesieo hes liing wits thef eristmdish panot venuld go commait.. [ftot swe_t that Pauthe hin a dough suant in trend thally is wabes a lewet I cous

{% include figure image_path="/assets/images/reddit/loss4.png" alt=""%}

Finally, a GRU model appears to learn faster than either the simple RNN or LSTM model, and have a far lower loss function by the 100th epoch. However, its actual output seems comparable to the other models. 

Example output:
> e ters for a thon bo chust intiiges way all Bnd and bewore. Thenascerusien. Yecarme that whet bathal nist to be whe Be somersinm follo cinpsciin. Itcone tisk!? Yele the Bodneven l atkinwt so woug that a buw ledo baving Mhat Ally fart a buthr on yha asde not makine o think wor haply bot commmont tore Shes dourd on yod onte bo heleibuy teandlise to gevaide bow heamd, now suas a not a jusp. Le a Bl’s

{% include figure image_path="/assets/images/reddit/loss5.png" alt=""%}
