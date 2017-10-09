---
layout: post
title: Listening for Orcas with an LSTM
---


## Inspiration

After completing a MOOC on Deep Learning I was anxious to find a data set to try out for my first project.  Recently Open AI demonstrated that sentiment could be detected by training a recurrent neural network (RNN) to make letter by letter predictions from a set of Amazon reviews.  The amazing thing about the research was that a single "neuron" was able to predict sentiment event though the model was trained on a data set that wasn't labeled.

Another paper came out around the same time about WaveNet where the authors were able to train a network on a combination of conditioning and raw audio data to create state of the art speech synthesis.  That network used a CNN based architecture, but similar results were produced from RNNs in another project called Sample RNN.

I wanted to try running raw audio data through an RNN and see if the outputs were able to detect the presence of marine mammals.  For a long time I was stuck trying to find a suitable data set until I found Orchive, a huge set of raw audio recordings of Orca whales.

## The Data Set

Orchive is a staggering set of raw audio data.  The total data set is over 20,000 hours collected over a period of 36 years.  The recordings are in mp3 format converted from audio tapes.  The tapes were recorded from two sites when Orcas were present, with researcher's voices also included at times giving context about when the tapes are turned on.

I scraped 200 mp3s from the website all from 2006.  The mp3s were converted to single track wav files to simplify the training process.  Each mp3 is about 55 minutes.  Note that this is less than 1% of the total data set but still much more than could practically train an LSTM 1 sample at a time.

## Design

## Training

## Results

## Next Steps

## References
1. [Unsupervised Sentiment Neuron](https://blog.openai.com/unsupervised-sentiment-neuron/) - Neat project that found a sentiment "neuron" in an RNN.
2. [WaveNet](https://deepmind.com/blog/wavenet-generative-model-raw-audio/) - State of the art voice results and nice music made by processing raw audio.
3. [Sample RNN](https://github.com/soroushmehr/sampleRNN_ICLR2017) - Similar results to Wavenet using RNNs.
4. [Orchive](http://orchive.cs.uvic.ca/) - Huge data set of raw audio recordings from orca whales.

![_config.yml]({{ site.baseurl }}/images/config.png)

The easiest way to make your first post is to edit this one. Go into /_posts/ and update the Hello World markdown file. For more instructions head over to the [Jekyll Now repository](https://github.com/barryclark/jekyll-now) on GitHub.
