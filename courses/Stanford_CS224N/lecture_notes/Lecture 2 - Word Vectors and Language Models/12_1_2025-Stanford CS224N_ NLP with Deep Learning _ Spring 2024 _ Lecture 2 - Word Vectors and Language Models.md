One of the interestings have emerged in Neural Network land, is it turns out that neural networks actually often work better when you throw some noise into the system, that having this noise in the system gives you jiggle and moves things around, and so, actually stochastic gradient descent, not only is way way way faster, but actually works better as a system for optimization of NN

  

And then we're going to go through each position in the corpus, using our estimates, we're going to try and predict the probability of words in the context, as we talked about last time, so that gives us an objective function from which we can then look at our errors, look at our gradient and update the vectors so that they learn to predict surrounding words better, and so the incredible thing is that we can do more than that, and we end up learning word vectors, which actually capture quite a lot of the semantics, the meaning and relationships between different words

  

**when we do the computation, considering the dot product between the various possible outside words with our center word, and we're using those to get a probability distribution over how likely the model thinks that different outside words were, and then we're comparing that to the actual outside word in the context, and that gives us the source of error -> so as such, this is what's referred to a bag of words model, that it doesn't actually know about the structure of sentences or even what's to the left and what's to the right, it's predicting exactly the same probabilities at each position to the left or the right, but it's wanting to know about what kind of words appear in the context of the center word**

  

  

﻿[10:38](https://youtu.be/nBor4jfWetQ?t=638)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_d53ff137-8a57-4b43-bcb4-b5119e875f1d.jpeg)

  

  

  

**f**or each word we have two vectors, in practice, we end up to average both of them, and really you find out that they end up very close, because if you think of it is going a long very position of the text, you're both going to be the case where if the text is the octopus has legs, you're going to have octopus in the center which the legs in the context, and a couple of time steps later, it's going to be legs in the center with octopus in the context

  

  

  

﻿[26:57](https://youtu.be/nBor4jfWetQ?t=1617)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_d361ed83-7710-43fe-911e-a485b722c442.jpeg)

  

  

If I make both the center vectors the same as the context vector for the same word, we end up with a quadratic term for one position X^2, and all other position is AX, and this makes the math messier

This doesn't make it better; it's just a mathematical point of view

  

Note, currently, softmax is not an expensive loss function, because of the new computers and it's high capability, but we will also introduce another loss function is negative sampling

  

﻿[29:03](https://youtu.be/nBor4jfWetQ?t=1743)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_8e38c6cf-f21e-47d1-acfe-3f16b9e68db0.jpeg)

  

  

for the dominator of the softmax function, we sum over every word in the vocabulary -- 400,000 words-- and take a dot product between 100 or 300 dimensions, a lot of math

  

So the idea of the negative sampling is to shortcut this and extract a true pair that is expected to output a high probability of similarity,

  

﻿[31:27](https://youtu.be/nBor4jfWetQ?t=1887)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_b75ad437-f6ea-485e-acb2-e99803f89e17.jpeg)

  

so we've got our center word and our actual context word, we would like this to be high probability--first term-- so since we are minimizing, we are going to negate that and have it go down, and then we're going to sample some other words, and we'd like this to be the opposite, but the other thing, that we have changed here is now we are not using the softmax anymore, we are using this sigmoid, so for the real outside world, we're hopping that this dot product is large, so it's probability is near 1, so that this will then help with the minimization, and for the other words, we'd their probability to be small --zero or near zero--

But as written, it was sort of sticking with the minus sign on the inside there, which works because this -- the sigmoid function--is symmetric, so you're wanting to be over here -- at zero probability-- which means if you negate it you'll be on the side which will be large

  

﻿[33:33](https://youtu.be/nBor4jfWetQ?t=2013)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_47145e0e-b004-41e7-b9c4-47f3580f3d02.jpeg)

  

we're going to pick a few words, it might only be five or 10 that are our negative samples, but for picking those words, what works well is not just to pick sort of randomly uniformly from all the 400,000 words in our vocab, what you basically want to do is be paying attention to how common the words are, so something like that is a really common word, And so we refer to that as the unigram distribution, that means you're sorf of just taking individual words independently, how commonly they are, so about 10% of the time, you' be choosing the, people have found that you can actually do even a bit better that that, to take the unigram dist and raising it to the power of 3/4-> less frequent words just become sampling more

  

so raising it to the 3/4 means that you're sort of somewhat upping the probability of the less frequent word, so you're sort of in between have every word uniform and exactly suing their relative frequencies in the text, you're sort of moving a little bit in the direction of uniform.

And so you get better results by going somewhat in the direction of sampling more uniformly, but you don't want to go all the way there, which would correspond to, I guess, putting a 0 in there rather than 3/4

  

  

  

﻿[34:09](https://youtu.be/nBor4jfWetQ?t=2049)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_4d370d1c-0bfa-405e-b035-782b7cc50e5f.jpeg)

  

﻿[35:04](https://youtu.be/nBor4jfWetQ?t=2104)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_b86d26cb-7d1f-455b-bdeb-7edebc99ca00.jpeg)

  

The size of the matrix will be 400,000 \* 400,000, which is a lot worse than our word vectors (400,000 times 100)

  

﻿[38:52](https://youtu.be/nBor4jfWetQ?t=2332)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_884f59e2-21c3-47b7-8966-50d18e0ecc25.jpeg)

  

  

If we want to have a smaller low low-dimensional vector, we know where the smaller singular vectors are, so we just set them to zero, and if we did that, then more of this goes away, and we end up with two-dimensional representations of our words, this had actually been explored before modern nerual word vectors, and using algorithm such as latent semantic analysis , and it is sort of half worked, but it never worked very well

These methods never worked very well

  

  

﻿[39:39](https://youtu.be/nBor4jfWetQ?t=2379)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_cd7622fb-804c-4c7b-bcec-0cbc90fc3bae.jpeg)

﻿[40:58](https://youtu.be/nBor4jfWetQ?t=2458)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_c6ed14bb-9e67-49c0-aa4c-2795bbff5b1a.jpeg)

  

  

We want a model in which linear components adding or subtracting a vector in a vector space correspond to a meaning difference. How can we do that -> ratios of co-occurrence probabilities

  

  

﻿[43:25](https://youtu.be/nBor4jfWetQ?t=2605)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_8b94088f-e2a4-4f64-bf3c-b63335847937.jpeg)

  

﻿[43:46](https://youtu.be/nBor4jfWetQ?t=2626)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_af0b8983-67e2-46aa-802c-8366b9c3e0b6.jpeg)

  

  

if you then look at the ratio of two of these co-occurrence probabilities, then what you get out is that for solid, it's for solid going to be large, and for gas, it's going to be small, and **so you're getting a direction in the space which will correspond to the solid liquid gas dimension of physics**, whereas for the other words, it will be about one

The straightforward way to turn the division into a linear one is to take the log function

  

  

﻿[44:49](https://youtu.be/nBor4jfWetQ?t=2689)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_253e01be-f62e-4c75-aa64-6a0eb1fc9abe.jpeg)

  

So you want to model this dot product such it's close to the log of the co-occurrence probability, but you do a little bit of extra work

  

  

﻿[46:37](https://youtu.be/nBor4jfWetQ?t=2797)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_9d82ed9d-e000-4f86-9e5c-d5ad5bd917d8.jpeg)

Intrinsic evaluation, you are doing a very specific internal subtask, and you just try and score whether it's good or bad, but they sor of distant from your downstream task, and improving the numbers internally may or may not help in your real task -- like summarization or question answering--

  

while in contrast, extrinsic are evaluations that internal modeling will help you with that real task or not

  

So for something like Word Vectors, if we just sort of measure, are they modeling word similarity? That's an intrinsic evaluation, but we'd probably like to know whether a model's word similarity well for some downstream task, which might be doing web search

  

in intrinsic word, what we can do is have a set of word analogies and find out which ones work, or with with human judgments of similarity, and then we get models of how well the scores are correlated between human judgments and our models judgments

﻿[48:41](https://youtu.be/nBor4jfWetQ?t=2921)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_0cd40f86-6631-4f2f-9f80-702b574cf0c9.jpeg)

  

﻿[49:57](https://youtu.be/nBor4jfWetQ?t=2997)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_45dd5cf7-4d56-41ab-958b-289674d7d18d.jpeg)

  

﻿[50:23](https://youtu.be/nBor4jfWetQ?t=3023)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_0f258857-645c-4858-b795-b28cf41cf1c2.jpeg)

  

  

﻿[50:57](https://youtu.be/nBor4jfWetQ?t=3057)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_bc1ffca5-9d83-49b8-b8c6-adef89bb7c18.jpeg)

  

﻿[52:11](https://youtu.be/nBor4jfWetQ?t=3131)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_002f493d-fa76-4690-8463-56744720f159.jpeg)

  

  

﻿[53:15](https://youtu.be/nBor4jfWetQ?t=3195)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_0f781419-4939-4948-b3c1-ab1852176a96.jpeg)

  

﻿[55:06](https://youtu.be/nBor4jfWetQ?t=3306)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_f33293ae-d5ef-43b3-993b-78b5d73bad0b.jpeg)

  

﻿[55:34](https://youtu.be/nBor4jfWetQ?t=3334)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_e78ec7da-587a-49f9-af04-2aadeed435d4.jpeg)

  

one way to deal with same word with different meanings, we're just going to say words have several meanings, and then we're going to take instances of words in text, we're going to cluster them based on their similarity of occurrence to decide which sense of the word to regard each token as, and then we're going to learn word vectors for those token clusters, which are our sensors

banl_1, bank_2, approach\__1, approach\__2, appraoach\_3, . ...

  

but that's not what's actually usually done these days, and instead what's usually done is you do only have one vector for jaguar, and when you do that, or pike here the one vector you learn is a **weighted average** of the vectors, that you would have learnt of the sensors

  

  

  

﻿[01:02:48](https://youtu.be/nBor4jfWetQ?t=3768)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_977b8e96-f6cc-462f-afeb-0aa7ae9cecc0.jpeg)

  

  

﻿[01:06:25](https://youtu.be/nBor4jfWetQ?t=3985)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_112973ef-c3cd-45f1-84f9-67d5aa354e9c.jpeg)

  

  

﻿[01:08:59](https://youtu.be/nBor4jfWetQ?t=4139)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_0c6e0b63-cef7-45c3-813e-f12f684c4ccf.jpeg)

in NN, our word vectors represent the actual words as symbols, and can move them around in the space, so that in terms in the original space, we've got a nonlinear classifier that can represent much more complex functions, but we will then use the word vectors to re-represent those words to do a final classification, **so at the end of our deep network, which we're about to build, we will have a linear classifier in terms of our represented vectors, but not in terms of our original space**

  

﻿[01:11:32](https://youtu.be/nBor4jfWetQ?t=4292)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_0bac8281-ded6-4861-b344-32fc67e80efc.jpeg)

at this point my classifier is going to be a linear classifier in terms of this internal representaion, that's used right at the end, but it's going to be a nonlinear classifier in trems of my word vectors

﻿[01:12:48](https://youtu.be/nBor4jfWetQ?t=4368)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_e28b4da9-ec57-4bd9-88bd-7792c0f8f272.jpeg)

  

  

﻿[01:16:24](https://youtu.be/nBor4jfWetQ?t=4584)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_c8c07961-309d-446c-914f-47ef11e1efec.jpeg)

  

﻿[01:18:19](https://youtu.be/nBor4jfWetQ?t=4699)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_252495f2-1ac1-4d69-be7f-c2b58137668e.jpeg)

  

﻿[01:18:42](https://youtu.be/nBor4jfWetQ?t=4722)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_6ab092fe-0d6c-4450-b974-7be507d77c60.jpeg)

  

So effectively, these intermediate layers let us learn a model that represents the input data in ways that will make it easier to classify, or easier to interpret and do things with downstream in our NN