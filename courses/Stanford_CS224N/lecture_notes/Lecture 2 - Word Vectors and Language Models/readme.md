# Neural Networks and Word Embeddings: A Comprehensive Guide

## Table of Contents

1. [Introduction to Neural Networks and Noise](#introduction-to-neural-networks-and-noise)
2. [Word Vector Learning and Prediction](#word-vector-learning-and-prediction)
3. [Bag of Words Model](#bag-of-words-model)
4. [Dual Vector Representation](#dual-vector-representation)
5. [Loss Functions and Computational Efficiency](#loss-functions-and-computational-efficiency)
   - [Softmax Computation Challenges](#softmax-computation-challenges)
   - [Negative Sampling](#negative-sampling)
6. [Sampling Strategies](#sampling-strategies)
7. [Matrix Representations and Dimensionality Reduction](#matrix-representations-and-dimensionality-reduction)
8. [GloVe: Modeling Co-occurrence Ratios](#glove-modeling-co-occurrence-ratios)
9. [Evaluation Methods](#evaluation-methods)
   - [Intrinsic vs Extrinsic Evaluation](#intrinsic-vs-extrinsic-evaluation)
10. [Word Sense Disambiguation](#word-sense-disambiguation)
11. [Neural Network Classifiers](#neural-network-classifiers)
12. [Deep Representations](#deep-representations)

---

## Introduction to Neural Networks and Noise

One of the interesting phenomena that has emerged in Neural Network research is that **neural networks actually often work better when you throw some noise into the system**. Having this noise in the system gives you jiggle and moves things around. Actually, **stochastic gradient descent** not only is way way way faster, but actually works better as a system for optimization of neural networks.

---

## Word Vector Learning and Prediction

The training process involves going through each position in the corpus. Using our estimates, we try and **predict the probability of words in the context**, as discussed previously. This gives us an objective function from which we can then:

- Look at our errors
- Look at our gradient
- Update the vectors so that they learn to predict surrounding words better

The incredible thing is that we can do more than that—we end up learning **word vectors which actually capture quite a lot of the semantics, the meaning and relationships between different words**.

---

## Bag of Words Model

When we do the computation, we consider the **dot product between the various possible outside words with our center word**. We use those to get a probability distribution over how likely the model thinks that different outside words were. Then we compare that to the actual outside word in the context, which gives us the source of error.

**As such, this is what's referred to as a bag of words model**—it doesn't actually know about the structure of sentences or even what's to the left and what's to the right. It's predicting exactly the same probabilities at each position to the left or the right, but it's wanting to know about what kind of words appear in the context of the center word.

**Graph 1:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_d53ff137-8a57-4b43-bcb4-b5119e875f1d.jpeg)

---

## Dual Vector Representation

For each word we have **two vectors**. In practice, we end up averaging both of them, and really you find out that they end up very close. If you think of it as going along every position of the text:

- When the text is *"the octopus has legs"*, you're going to have **octopus** in the center with **legs** in the context
- A couple of time steps later, it's going to be **legs** in the center with **octopus** in the context

### Mathematical Considerations

If I make both the center vectors the same as the context vector for the same word, we end up with:
- A **quadratic term** for one position: X²
- All other positions: AX

This makes the math messier. **This doesn't make it better; it's just a mathematical point of view.**

---

## Loss Functions and Computational Efficiency

**Note:** Currently, softmax is not an expensive loss function because of new computers and their high capability, but we will also introduce another loss function called **negative sampling**.

**Graph 2:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_d361ed83-7710-43fe-911e-a485b722c442.jpeg)

### Softmax Computation Challenges

**Graph 3:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_8e38c6cf-f21e-47d1-acfe-3f16b9e68db0.jpeg)

For the denominator of the softmax function, we sum over **every word in the vocabulary**—400,000 words—and take a dot product between 100 or 300 dimensions. That's **a lot of math**.

### Negative Sampling

The idea of negative sampling is to **shortcut this and extract a true pair that is expected to output a high probability of similarity**.

**Graph 4:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_b75ad437-f6ea-485e-acb2-e99803f89e17.jpeg)

Here's how it works:

- We've got our **center word** and our **actual context word**—we would like this to be high probability *(first term)*
- Since we are minimizing, we negate that and have it go down
- Then we sample some other words, and we'd like this to be the opposite
- We've changed from softmax to using **sigmoid**

For the real outside word:
- We're hoping that this dot product is large
- Its probability is near 1
- This will then help with the minimization

For the other words:
- We'd like their probability to be small (zero or near zero)

As written, it sticks with the minus sign on the inside there, which works because the **sigmoid function is symmetric**. You're wanting to be at zero probability, which means if you negate it you'll be on the side which will be large.

**Graph 5:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_47145e0e-b004-41e7-b9c4-47f3580f3d02.jpeg)

---

## Sampling Strategies

We're going to pick a few words—it might only be **5 or 10** that are our negative samples. For picking those words, what works well is **not just to pick randomly uniformly** from all the 400,000 words in our vocabulary.

What you basically want to do is **pay attention to how common the words are**:

- Something like "the" is a really common word
- We refer to that as the **unigram distribution**—taking individual words independently based on how commonly they appear
- About 10% of the time, you'd be choosing "the"

### Enhanced Sampling Formula

People have found that you can actually do even a bit better by taking the unigram distribution and **raising it to the power of 3/4**. This means less frequent words become sampled more.

**Benefits:**
- Somewhat ups the probability of less frequent words
- You're between having every word uniform and exactly using their relative frequencies in the text
- Moving a little bit in the direction of uniform sampling

You get **better results** by going somewhat in the direction of sampling more uniformly, but you don't want to go all the way there (which would correspond to putting a 0 in there rather than 3/4).

**Graph 6:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_4d370d1c-0bfa-405e-b035-782b7cc50e5f.jpeg)

**Graph 7:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_b86d26cb-7d1f-455b-bdeb-7edebc99ca00.jpeg)

---

## Matrix Representations and Dimensionality Reduction

The size of the matrix will be **400,000 × 400,000**, which is a lot worse than our word vectors (400,000 × 100).

**Graph 8:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_884f59e2-21c3-47b7-8966-50d18e0ecc25.jpeg)

If we want to have a smaller low-dimensional vector, we know where the smaller singular vectors are, so we just set them to zero. If we did that, then more of this goes away, and we end up with **two-dimensional representations of our words**.

This had actually been explored before modern neural word vectors using algorithms such as **Latent Semantic Analysis (LSA)**. It sort of half worked, but **these methods never worked very well**.

**Graph 9:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_cd7622fb-804c-4c7b-bcec-0cbc90fc3bae.jpeg)

**Graph 10:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_c6ed14bb-9e67-49c0-aa4c-2795bbff5b1a.jpeg)

---

## GloVe: Modeling Co-occurrence Ratios

We want a model in which **linear components**—adding or subtracting a vector in a vector space—correspond to a meaning difference. How can we do that? → **Ratios of co-occurrence probabilities**

**Graph 11:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_8b94088f-e2a4-4f64-bf3c-b63335847937.jpeg)

**Graph 12:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_af0b8983-67e2-46aa-802c-8366b9c3e0b6.jpeg)

If you look at the **ratio of two co-occurrence probabilities**:

- For **solid**, it's going to be large
- For **gas**, it's going to be small
- **You're getting a direction in the space which will correspond to the solid-liquid-gas dimension of physics**
- For other words, it will be about one

The straightforward way to turn the division into a linear one is to **take the log function**.

**Graph 13:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_253e01be-f62e-4c75-aa64-6a0eb1fc9abe.jpeg)

You want to model this dot product such that it's close to the **log of the co-occurrence probability**, but you do a little bit of extra work.

**Graph 14:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_9d82ed9d-e000-4f86-9e5c-d5ad5bd917d8.jpeg)

---

## Evaluation Methods

### Intrinsic vs Extrinsic Evaluation

**Intrinsic evaluation:** You are doing a very specific internal subtask, and you just try and score whether it's good or bad. But they're sort of distant from your downstream task, and improving the numbers internally may or may not help in your real task (like summarization or question answering).

**Extrinsic evaluation:** In contrast, these are evaluations of whether internal modeling will help you with that real task or not.

### Examples

For something like **Word Vectors**:
- If we just measure "are they modeling word similarity?"—that's an **intrinsic evaluation**
- We'd probably like to know whether a model's word similarity works well for some **downstream task**, which might be doing web search

In intrinsic evaluation, what we can do is:
- Have a set of **word analogies** and find out which ones work
- Compare with **human judgments of similarity**
- Get models of how well the scores are correlated between human judgments and our model's judgments

**Graph 15:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_0cd40f86-6631-4f2f-9f80-702b574cf0c9.jpeg)

**Graph 16:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_45dd5cf7-4d56-41ab-958b-289674d7d18d.jpeg)

**Graph 17:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_0f258857-645c-4858-b795-b28cf41cf1c2.jpeg)

**Graph 18:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_bc1ffca5-9d83-49b8-b8c6-adef89bb7c18.jpeg)

**Graph 19:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_002f493d-fa76-4690-8463-56744720f159.jpeg)

---

## Word Sense Disambiguation

**Graph 20:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_0f781419-4939-4948-b3c1-ab1852176a96.jpeg)

**Graph 21:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_f33293ae-d5ef-43b3-993b-78b5d73bad0b.jpeg)

**Graph 22:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_e78ec7da-587a-49f9-af04-2aadeed435d4.jpeg)

One way to deal with the same word having different meanings:

1. We're just going to say **words have several meanings**
2. We take instances of words in text
3. We **cluster them based on their similarity of occurrence** to decide which sense of the word to regard each token as
4. We learn word vectors for those token clusters, which are our senses

Examples: `bank_1`, `bank_2`, `approach_1`, `approach_2`, `approach_3`, etc.

### Current Practice

But that's not what's actually usually done these days. Instead, what's usually done is:

- You do only have **one vector** for "jaguar" (or "pike")
- When you do that, the one vector you learn is a **weighted average** of the vectors that you would have learned for the individual senses

---

## Neural Network Classifiers

**Graph 23:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_977b8e96-f6cc-462f-afeb-0aa7ae9cecc0.jpeg)

**Graph 24:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_112973ef-c3cd-45f1-84f9-67d5aa354e9c.jpeg)

**Graph 25:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_0c6e0b63-cef7-45c3-813e-f12f684c4ccf.jpeg)

In neural networks, our **word vectors represent the actual words as symbols** and can move them around in the space. In terms of the original space, we've got a **nonlinear classifier** that can represent much more complex functions.

But we will then use the word vectors to re-represent those words to do a final classification. **So at the end of our deep network, which we're about to build, we will have a linear classifier in terms of our represented vectors, but not in terms of our original space.**

**Graph 26:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_0bac8281-ded6-4861-b344-32fc67e80efc.jpeg)

At this point, my classifier is going to be a **linear classifier in terms of this internal representation** that's used right at the end, but it's going to be a **nonlinear classifier in terms of my word vectors**.

**Graph 27:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_e28b4da9-ec57-4bd9-88bd-7792c0f8f272.jpeg)

---

## Deep Representations

**Graph 28:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_c8c07961-309d-446c-914f-47ef11e1efec.jpeg)

**Graph 29:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_252495f2-1ac1-4d69-be7f-c2b58137668e.jpeg)

**Graph 30:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_6ab092fe-0d6c-4450-b974-7be507d77c60.jpeg)

**Effectively, these intermediate layers let us learn a model that represents the input data in ways that will make it easier to classify, or easier to interpret and do things with downstream in our neural network.**