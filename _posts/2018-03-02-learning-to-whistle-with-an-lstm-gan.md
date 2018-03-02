---
layout: post
title: Learning to whistle with an LSTM GAN
excerpt_separator: <!--more-->
---

![GAN LSTM generated orca whistle]({{"/images/generated-whistle.png" | asset_url}})

For my first deep learning project, I tried pumping raw audio into an LSTM and looked for neurons that matched Orca whistles.  For my next project I wanted to try using a GAN architecture and spectrograms to find stronger abstractions.

<!--more-->

When training with one raw audio sample at a time, there were neurons that matched the immediate sound of whistles, but not higher level abstractions like the general presence of whales, human speakers, or white noise.  The goal of the new network was to detect these higher level abstractions.

## The Data Set

This project used the same data set: Orchive - a staggering set of raw audio data.  The total data set is over 20,000 hours collected over a period of 36 years.  The recordings are in mp3 format converted from audio tapes.  The tapes were recorded from two sites when orcas were present, with researcher's voices also included at times giving context about when the tapes were turned on.

I scraped 200 mp3s from the website, all recorded in 2006.  The mp3s were converted to single track wav files to simplify the training process.  Each recording is about 55 minutes.  Note that this is less than 1% of the total data.

## Design

The network preprocesses the raw audio into 128x128 spectrograms with each image representing 3 seconds of audio.  The spectrograms are fed into a DCGAN implementation.

The GAN implementation is modified to incorporate recurrent networks.  For the generator, z is fed into an LSTM and both the LSTM output and z are fed into the generator instead of just z.  The discriminator feeds the output of the convolutions into a separate LSTM network and uses that LSTM output to make its prediction.

## Training

Training of the network is done by sampling random series of spectrograms from the raw data and repeating the standard GAN training for the sequence of data.  So the generator only has its LSTM to remember what it previously generated and the discriminator repeatedly guesses based on what it sees and the state of its separate LSTM.  (An alternative LSTM GAN architecture feeds a series of data to both the generator and discriminator so the generator bases its image on real data.)

![Sample of raw training data]({{"/images/scrygan-training-sample.png" | asset_url}})

This sample shows what the raw data looks like.

## Results

The generator was able to create decent sprectrograms that matched high level abstractions.  Inspecting the generated samples reveals the generator creating orca whistles, human speech, white noise, and other patterns matching the raw data.  One limitation is that the generated image creates the whistles in the same location repeatedly and only made slight variations in shape.  I'm not sure if this is inherent in the nature of convolutional networks or indicates a flaw in the implementation.

![Generated spectrogram sample]({{"/images/scrygan-generator-7740.png" | asset_url}})

A pattern in the generator was human speech appearing to change to orca whistles.  This could be an idiosyncrasy of the generator or it could show the network learning the pattern of field workers giving context when the tapes are turned on because whales are present.

![Human to orca 1]({{"/images/scrygan-human-to-orca-1.png" | asset_url}})

![Human to orca 2]({{"/images/scrygan-human-to-orca-2.png" | asset_url}})

![Human to orca 3]({{"/images/scrygan-human-to-orca-3.png" | asset_url}})

## Applications

It has been shown that machine learning techniques can be used to classify high level behavior patterns in marine mammals.  Perhaps a network could be built to detect distressed marine mammals. Given that so many marine mammals are endangered, detecting marine mammals that need help could be an important step to help prevent extinction.  For example the vaquita porpoise has a dwindling population with perhaps less than 20 left, and other species of marine mammals are endangered all over the world.

Note that going from generated spectrograms back to audio probably wouldn't create great results, the best potential is using the discriminator to create something like embeddings of raw audio and finding applications from that.

## Next Steps

I'm currently debating what to do for my next deep learning project.  I'm most interested in unsupervised learning, but I might try setting up a supervised project to show more objective numbers.  Alternatively I might dive deeper into this model and do some analysis of the neurons to see if there are highly predictive neurons, or see if the model can be improved to create more accurate whistles.

## References/further reading

1. [The behavioral context of common dolphin (Delphinus sp.) vocalizations](onlinelibrary.wiley.com/doi/10.1111/j.1748-7692.2011.00498.x/abstract) - Paper showing how machine learning techniques can classify marine mammal behavior
2. [DCGAN and spectrograms](http://deepsound.io/dcgan_spectrograms.html) Deepsound post about using DCGAN to create spectrograms
3. [Vaquita CPR](https://www.vaquitacpr.org/) Efforts to save the dwindling vaquita porpoise population

## More generated samples

![Generated spectrogram sample]({{"/images/scrygan-generator-3110.png" | asset_url}})
![Generated spectrogram sample]({{"/images/scrygan-generator-4230.png" | asset_url}})
![Generated spectrogram sample]({{"/images/scrygan-generator-4430.png" | asset_url}})
![Generated spectrogram sample]({{"/images/scrygan-generator-5500.png" | asset_url}})
![Generated spectrogram sample]({{"/images/scrygan-generator-7740.png" | asset_url}})
![Generated spectrogram sample]({{"/images/scrygan-generator-7990.png" | asset_url}})
![Generated spectrogram sample]({{"/images/scrygan-generator-8420.png" | asset_url}})
![Generated spectrogram sample]({{"/images/scrygan-generator-9560.png" | asset_url}})
![Generated spectrogram sample]({{"/images/scrygan-generator-9570.png" | asset_url}})
