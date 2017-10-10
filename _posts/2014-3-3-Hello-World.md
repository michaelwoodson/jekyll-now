---
layout: post
title: Listening for Orcas with an LSTM
---


## Inspiration

![Photo shared by Minette Layne]({{ site.baseurl }}/images/orca.jpg)

After completing a course on Deep Learning I was anxious to find a data set to try out for my first project.  Recently Open AI demonstrated that sentiment could be detected by training a recurrent neural network (RNN) to make letter by letter predictions in a set of Amazon reviews.  The amazing thing about the research was that a single "neuron" was able to predict sentiment even though the model was trained on a data set that wasn't labeled.

Another paper came out around the same time about WaveNet where the authors were able to train a network on a combination of conditioning and raw audio data to create state of the art speech synthesis.  That network used a CNN based architecture, but similar results were produced from RNNs in another project called Sample RNN.

I wanted to try running raw audio data through an RNN and see if the outputs were able to detect the presence of marine mammals.  For a long time I was stuck trying to find a suitable data set until I found Orchive, a huge set of raw audio recordings of orca whales.

## The Data Set

Orchive is a staggering set of raw audio data.  The total data set is over 20,000 hours collected over a period of 36 years.  The recordings are in mp3 format converted from audio tapes.  The tapes were recorded from two sites when orcas were present, with researcher's voices also included at times giving context about when the tapes are turned on.

I scraped 200 mp3s from the website, all recorded in 2006.  The mp3s were converted to single track wav files to simplify the training process.  Each recording is about 55 minutes.  Note that this is less than 1% of the total data set but still much more than could practically train an LSTM 1 sample at a time with my hardware (a single GTX 1080).

## Design

The network feeds a window of 16 samples into an LSTM.  The LSTM output is stacked with the raw input and fed into two fully connected layers.  The final output is mu law encoded as recommended in the WaveNet paper so training on the next sample can be done via softmax cross entropy.

## Training

For training, 128 of the 200 recordings are chosen at random.  Ten seconds samples are then chosen randomly from within those recording and fed in mini-batches of size 32 for truncated back propagation through time (TBPTT).  For this unsupervised learning no validation or test set was used, the network was just repeatedly fed training data till the loss stabilized.

For a baseline, the LSTM was ommitted and only two fully connected layers were used.  This baseline was used to ensure that the LSTM was participating in the network.

## Results

# Notes

* Tried a multiplicative LSTM like in the Open AI paper and also tried the TensorFlow layer normalization implementation.  In practice those cells performance was too slow compared to the optimized BlockLSTMCell to provide any benefit.  It would be nice if BlockLSTMCell included those options.
* Using more than 1 layer of LSTM (per timestep) with MultiRNNCell performed very poorly.  Perhaps I made a mistake passing the states between mini-batches.  The Sample RNN paper suggested that a 2 layer RNN should have comparable performance.

## Next Steps



## References
1. [Unsupervised Sentiment Neuron](https://blog.openai.com/unsupervised-sentiment-neuron/) - Neat project that found a sentiment "neuron" in an RNN.
2. [WaveNet](https://deepmind.com/blog/wavenet-generative-model-raw-audio/) - State of the art voice results and nice music made by processing raw audio.
3. [Sample RNN](https://github.com/soroushmehr/sampleRNN_ICLR2017) - Similar results to Wavenet using RNNs.
4. [Orchive](http://orchive.cs.uvic.ca/) - Huge data set of raw audio recordings from orca whales.
