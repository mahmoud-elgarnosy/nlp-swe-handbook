a Knowledge side of languages, and how that's made humans beings great:

  

1\. The language has also allowed humans, I'd argue, to achieve a higher level of thought, most of the time when we're doing hieher level congnition, if you're thinking to yourself, "Oh, my friend seemed upset about what I said last night, I should probably work out who to fix that, or maybe I coluld blah blah blah blah", I think we think in language and plan out things, so that it's given us a scaffolding to do much more detailed thought and planning

  

2- writting is really, really recent, I mean no one really knows old human languages are, but writting we do know, writting is really recent, is about 5000 years old, writing proved to be this amazing congnitive tool that just gave humanity an enormous leg up, because suddenly, it's not only that you could share information, and learn from the people that were standing withing 50 feet of you, you could then share knowledge across time and space, thanks to writing we are now in all these technologies that we walk around with today in just very shor amount of time

  

"The common misconception is that language use has primarily to do with words and what they mean. it doesn't. it has primarily to do with people and what they mean."

  

The difference between human & AI is that we have language--language is not words, words tend to change every generation of Gen Z, and language can be extended. We take steps and actions by involving the language--, but AI doesn't have language, but numbers

  

The language has allowed humans to achieve a higher level of thought and planning

﻿[](https://youtu.be/DzpHeXVSC5I?t=1279)﻿[21:19](https://youtu.be/DzpHeXVSC5I?t=1279.319393)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_b0fbed66-e278-424f-b9ed-86c364e53203.jpeg)

  

GPT-2 is the first model that shows the ability of LLM, It can produce grammatically correct sentences, not garbage, and is also can also understand a lot of things, for example, it knows if something is stolen, it's a theft

  

Recent models are multimodal and can operate across modes

  

word representations:

WordNet: a fancy thesaurus that showed word relations, so it would tell you about synonyms, and is the kind of things--hypernyms--, for eg, Panda is the kind of carnivore, which is a placental, which is a mammal, and things like that

﻿[](https://youtu.be/DzpHeXVSC5I?t=1807)﻿[30:07](https://youtu.be/DzpHeXVSC5I?t=1807.95741)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_2559d94e-65c5-480a-9581-53ce50874ba1.jpeg)

  

But systems like Wordnet were never very good for computational meaning; they missed a lot of nuance

﻿[](https://youtu.be/DzpHeXVSC5I?t=1883)﻿[31:23](https://youtu.be/DzpHeXVSC5I?t=1883.381989)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_58d73062-4afe-41ae-aa41-a66abbcda9e5.jpeg)

  

  

Another representation is one-hot vectors -denotational semantics-- that indicate the position of the word, and no indication of the meaning or similarity between words

﻿[](https://youtu.be/DzpHeXVSC5I?t=1983)﻿[33:03](https://youtu.be/DzpHeXVSC5I?t=1983.699036)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_d3b7afa0-0e06-4b10-824d-7d0574bb67d6.jpeg)

  

  

Distributional semantics--word vectors--: Another representation is a context representation

We are going to use statistics about words and what other words appear around them, to learn a new kind of representation of a word

  

﻿[](https://youtu.be/DzpHeXVSC5I?t=2172)﻿[36:12](https://youtu.be/DzpHeXVSC5I?t=2172.883486)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_b69b075b-b18c-42b9-a8c4-bbc113cea9bf.jpeg)

  

The first step is to learn a good word vector for different words that gives us a good sense of the meaning of words

  

We refer to them as embedding because we can think of this as a vector in a high-dimensional space, and so we're embedding each word as a position in that high-dimensional space

﻿[](https://youtu.be/DzpHeXVSC5I?t=2225)﻿[37:05](https://youtu.be/DzpHeXVSC5I?t=2225.700009)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_e41f150d-899a-46d0-98cc-3b74d7416b0e.jpeg)

  

So we get these interesting semantic spaces, where things that have similar meaning are close by to each other. It learns the representation of a word from a large corpus of sentences. Each sentence does something to change the representation of a word in some way, until it gives the final representation o

﻿[](https://youtu.be/DzpHeXVSC5I?t=2414)﻿[40:14](https://youtu.be/DzpHeXVSC5I?t=2414.985074)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_9dc6a90f-302b-4a5c-aab5-c190637a5086.jpeg)

"what we are doing at the moment, each word has a single embedding, and what you can think of that embedding as an average over all its sensors, like a word bank, could it be river bank or financial institution bank?"

  

There are many ways to do so; the first one is that we will introduce it today, which is Word2vec.

\`Corpus is a Latin word of body, and its plural is corpora.\`

  

What we are doing there is that we use the similarity of the word vectors for context c and the outside words to calculate the probability that they should have occurred or not, and we just keep fiddling, and we learn word vectors

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_3d41a607-1976-41dd-aea8-dd652cbbb6b3.jpeg)

  

﻿[](https://youtu.be/DzpHeXVSC5I?t=2475)﻿[41:15](https://youtu.be/DzpHeXVSC5I?t=2475.149527)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_ef21f21b-e7a1-4321-b5d6-5e6f73ca47cb.jpeg)

  

  

  

a vector of each word type:

A Word type means the word \`problems\` wherever it occurs, which is differentiated from a word token, which is this instance -one instance-- of the word \`problems.\`

  

We want the probability of co-occurrence to be high for words that actually do occur within the nearby context of each other

  

And so what we want to do is come up with vector representations of words that will let us predict these probabilities well, what likely comes after this word, and what it doesn't

﻿[](https://youtu.be/DzpHeXVSC5I?t=3124)﻿[52:04](https://youtu.be/DzpHeXVSC5I?t=3124.628892)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_311b43c4-f0fa-486f-87c6-074feafb2a56.jpeg)

  

  

MLE : maximize this likelihood, where we're going through every poistion in the text, and then we're going through every word in the context and sort of wanting to make this big

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_1c4320cc-3560-4f66-8a86-d861082d54e8.jpeg)

  

For no reason everyone got into minimizing things rather than maximizing things, and so the algorithms that we use get referred to as gradient descent

1 - Rather than working with enormous products, we compute the log that converts those products into sums -> negative log likelihood

2 - Since we will git bigger depending on the number of words in the corpus, so we normalize things ->>> our objective function is the average negative log likelihood

  

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_317b94d3-a370-434a-a51b-f679ac8ee8cf.jpeg)

  

﻿[](https://youtu.be/DzpHeXVSC5I?t=3252)﻿[54:12](https://youtu.be/DzpHeXVSC5I?t=3252.093204)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_fffa7c63-2154-4a1b-89d6-b74591064140.jpeg)

  

How do we calculate these probabilities -> we are going to define this probability in terms of the word vector, so we're going to say each word type is represented by a vector of real numbers, and we're going to have a formula that works out the probability simply in terms of the vectors of each word, that there are no ohter parameters in this model

  

To calculate probabilities out of vectors, the first thing must comes to your mind is to take a softmax function

  

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_0f7315ec-c2ce-4aa4-95d5-7c8d58fe170e.jpeg)

  

﻿[](https://youtu.be/DzpHeXVSC5I?t=3356)﻿[55:56](https://youtu.be/DzpHeXVSC5I?t=3356.519272)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_685c4dca-9d4d-4bc9-905d-30bd793d9200.jpeg)

  

All and only the parameters of our model are these word vectors for each word in the vocabulary. That's a lot of parameters because we have a lot of words, and we've got fairly big word vectors, but they are the only parameters

  

dot product of the two word vectors, if it is high, they'll be similar, and therefore, they'll have a high probability

'The' also has to be similar to basically any noun

  

﻿[](https://youtu.be/DzpHeXVSC5I?t=3510)﻿[58:30](https://youtu.be/DzpHeXVSC5I?t=3510.706873)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_5fa523c2-7619-49f9-9d7e-0fd7ca4b210d.jpeg)

  

We took the softmax function for the dot product to output the probability

We're going to start off with random vectors for every word, and then we want to fiddle those vectors, so that the calculated probabilities of words in a context go up, and we're going to keep fiddling until they stop going up anymore, and we're getting the highest probability estimates that we can

  

  

  

﻿[](https://youtu.be/DzpHeXVSC5I?t=3785)﻿[01:03:05](https://youtu.be/DzpHeXVSC5I?t=3785.969892)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_32c52f05-b569-4bf0-bd94-a4afa2bc038f.jpeg)

  

So if we have a vocabulary of 400,000 words and 100-dimensional word vectors, that means we'd have 400,000 times 2 times 100, we'd have 80 million parameters, to optimize

  

This feels like magic, I mean, it doesn't really seem like we could just start with nothing, we could start with random word vectors, and a pile of text, and say, do some math, and we will get something useful out, but the miracle of what happens in these deep learning spaces is we do get something useful out k

  
  

  

Takes the actual observed u vector, and it compares it to the weighted prediction, the weighted sum of our current ux vectors, based on how likely they were to occur

And so what you'd like to have is your expectation, the weighted average, to be the same as what was observed, because then you'll get a derivative of 0, which means that you've hit a maximum

This is the formula of partial derivative with respect to center vector parameters, you can also go with outside vector parameters