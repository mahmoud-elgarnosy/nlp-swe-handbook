# Language, Thought, and Word Embeddings: Understanding Human Cognition and AI

## Table of Contents

1. [The Power of Language in Human Cognition](#the-power-of-language-in-human-cognition)
   - [Language as a Cognitive Tool](#language-as-a-cognitive-tool)
   - [The Revolutionary Impact of Writing](#the-revolutionary-impact-of-writing)
2. [Understanding Language Beyond Words](#understanding-language-beyond-words)
3. [The Difference Between Human and AI Language](#the-difference-between-human-and-ai-language)
4. [The Evolution of Language Models](#the-evolution-of-language-models)
5. [Word Representation Systems](#word-representation-systems)
   - [WordNet: The Thesaurus Approach](#wordnet-the-thesaurus-approach)
   - [One-Hot Vectors: Denotational Semantics](#one-hot-vectors-denotational-semantics)
   - [Distributional Semantics: Word Vectors](#distributional-semantics-word-vectors)
6. [Word Embeddings and Semantic Spaces](#word-embeddings-and-semantic-spaces)
7. [Word2vec: Learning from Context](#word2vec-learning-from-context)
   - [Word Types vs Word Tokens](#word-types-vs-word-tokens)
   - [Predicting Co-occurrence Probabilities](#predicting-co-occurrence-probabilities)
8. [Maximum Likelihood Estimation (MLE)](#maximum-likelihood-estimation-mle)
9. [Computing Probabilities from Vectors](#computing-probabilities-from-vectors)
   - [The Softmax Function](#the-softmax-function)
   - [Model Parameters](#model-parameters)
10. [Training Word Vectors](#training-word-vectors)
11. [The Scale of Word Vector Models](#the-scale-of-word-vector-models)
12. [The Magic of Deep Learning](#the-magic-of-deep-learning)
13. [Gradient Descent and Optimization](#gradient-descent-and-optimization)

---

## The Power of Language in Human Cognition

### Language as a Cognitive Tool

**1. Language enables higher-level thought and planning:**

The language has allowed humans to achieve a higher level of thought. Most of the time when we're doing higher-level cognition—if you're thinking to yourself, *"Oh, my friend seemed upset about what I said last night, I should probably work out how to fix that, or maybe I could blah blah blah blah"*—**I think we think in language and plan out things**. It has given us a **scaffolding to do much more detailed thought and planning**.

### The Revolutionary Impact of Writing

**2. Writing as humanity's cognitive amplifier:**

Writing is really, really recent. I mean, no one really knows how old human languages are, but writing we do know—**writing is really recent, about 5,000 years old**. 

Writing proved to be this **amazing cognitive tool that just gave humanity an enormous leg up** because suddenly:
- It's not only that you could share information and learn from the people standing within 50 feet of you
- You could then **share knowledge across time and space**

Thanks to writing, we are now in all these technologies that we walk around with today in just a very short amount of time.

---

## Understanding Language Beyond Words

> **"The common misconception is that language use has primarily to do with words and what they mean. It doesn't. It has primarily to do with people and what they mean."**

---

## The Difference Between Human and AI Language

The difference between humans and AI is that **we have language**—language is not words. Words tend to change every generation (Gen Z, for example), and **language can be extended**. We take steps and actions by involving language, **but AI doesn't have language, but numbers**.

The language has allowed humans to achieve a higher level of thought and planning.

**Graph 1:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_b0fbed66-e278-424f-b9ed-86c364e53203.jpeg)

---

## The Evolution of Language Models

**GPT-2** is the first model that shows the ability of LLMs. It can:
- Produce **grammatically correct sentences**, not garbage
- Understand a lot of things (for example, it knows if something is stolen, it's a theft)

Recent models are **multimodal** and can operate across modes.

---

## Word Representation Systems

### WordNet: The Thesaurus Approach

**WordNet** is a fancy thesaurus that showed word relations. It would tell you about:
- **Synonyms**
- **Hypernyms** (kind-of relationships)

For example: *Panda* is a kind of *carnivore*, which is a *placental*, which is a *mammal*, and things like that.

**Graph 2:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_2559d94e-65c5-480a-9581-53ce50874ba1.jpeg)

But systems like WordNet were **never very good for computational meaning**—they missed a lot of nuance.

**Graph 3:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_58d73062-4afe-41ae-aa41-a66abbcda9e5.jpeg)

### One-Hot Vectors: Denotational Semantics

Another representation is **one-hot vectors** (denotational semantics) that indicate the position of the word, with **no indication of the meaning or similarity between words**.

**Graph 4:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_d3b7afa0-0e06-4b10-824d-7d0574bb67d6.jpeg)

### Distributional Semantics: Word Vectors

**Distributional semantics (word vectors):** Another representation is a context representation.

We are going to use **statistics about words and what other words appear around them** to learn a new kind of representation of a word.

**Graph 5:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_b69b075b-b18c-42b9-a8c4-bbc113cea9bf.jpeg)

---

## Word Embeddings and Semantic Spaces

The first step is to **learn a good word vector for different words** that gives us a good sense of the meaning of words.

We refer to them as **embeddings** because we can think of this as a vector in a high-dimensional space, and so we're embedding each word as a position in that high-dimensional space.

**Graph 6:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_e41f150d-899a-46d0-98cc-3b74d7416b0e.jpeg)

So we get these **interesting semantic spaces**, where things that have similar meaning are close by to each other. It learns the representation of a word from a large corpus of sentences. Each sentence does something to change the representation of a word in some way, until it gives the final representation.

**Graph 7:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_9dc6a90f-302b-4a5c-aab5-c190637a5086.jpeg)

> **"What we are doing at the moment, each word has a single embedding, and what you can think of that embedding as an average over all its senses, like a word 'bank'—could it be river bank or financial institution bank?"**

---

## Word2vec: Learning from Context

There are many ways to do so; the first one we will introduce today is **Word2vec**.

> *Corpus is a Latin word for body, and its plural is corpora.*

What we are doing is that we **use the similarity of the word vectors for context c and the outside words to calculate the probability** that they should have occurred or not, and we just keep fiddling, and we learn word vectors.

**Graph 8:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_3d41a607-1976-41dd-aea8-dd652cbbb6b3.jpeg)

**Graph 9:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_ef21f21b-e7a1-4321-b5d6-5e6f73ca47cb.jpeg)

### Word Types vs Word Tokens

**A vector of each word type:**

- A **word type** means the word "problems" wherever it occurs
- This is differentiated from a **word token**, which is this instance (one instance) of the word "problems"

### Predicting Co-occurrence Probabilities

We want the **probability of co-occurrence to be high** for words that actually do occur within the nearby context of each other.

And so what we want to do is come up with **vector representations of words that will let us predict these probabilities well**—what likely comes after this word, and what doesn't.

**Graph 10:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_311b43c4-f0fa-486f-87c6-074feafb2a56.jpeg)

---

## Maximum Likelihood Estimation (MLE)

**MLE:** Maximize this likelihood, where we're going through every position in the text, and then we're going through every word in the context and wanting to make this big.

**Graph 11:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_1c4320cc-3560-4f66-8a86-d861082d54e8.jpeg)

For no reason, everyone got into **minimizing things rather than maximizing things**, and so the algorithms that we use get referred to as **gradient descent**.

**Two key transformations:**

1. Rather than working with enormous products, we compute the **log** that converts those products into sums → **negative log likelihood**

2. Since we will get bigger depending on the number of words in the corpus, we normalize things → our objective function is the **average negative log likelihood**

**Graph 12:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_317b94d3-a370-434a-a51b-f679ac8ee8cf.jpeg)

**Graph 13:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_fffa7c63-2154-4a1b-89d6-b74591064140.jpeg)

---

## Computing Probabilities from Vectors

How do we calculate these probabilities? We are going to **define this probability in terms of the word vector**. So we're going to say:
- Each word type is represented by a **vector of real numbers**
- We're going to have a formula that works out the probability simply in terms of the vectors of each word
- **There are no other parameters in this model**

### The Softmax Function

To calculate probabilities out of vectors, the first thing that must come to your mind is to take a **softmax function**.

**Graph 14:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_0f7315ec-c2ce-4aa4-95d5-7c8d58fe170e.jpeg)

**Graph 15:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_685c4dca-9d4d-4bc9-905d-30bd793d9200.jpeg)

### Model Parameters

**All and only the parameters of our model are these word vectors for each word in the vocabulary.** That's a lot of parameters because:
- We have a lot of words
- We've got fairly big word vectors
- But they are the **only parameters**

**How it works:**
- **Dot product** of the two word vectors: if it is high, they'll be similar, and therefore, they'll have a high probability
- "The" also has to be similar to basically any noun

**Graph 16:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_5fa523c2-7619-49f9-9d7e-0fd7ca4b210d.jpeg)

---

## Training Word Vectors

We took the **softmax function for the dot product** to output the probability.

**The training process:**
1. We're going to start off with **random vectors** for every word
2. Then we want to **fiddle those vectors**, so that the calculated probabilities of words in a context go up
3. We're going to keep fiddling until they stop going up anymore
4. We're getting the **highest probability estimates** that we can

**Graph 17:**  
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_32c52f05-b569-4bf0-bd94-a4afa2bc038f.jpeg)

---

## The Scale of Word Vector Models

So if we have:
- A vocabulary of **400,000 words**
- **100-dimensional word vectors**

That means we'd have:
- 400,000 × 2 × 100
- **80 million parameters to optimize**

---

## The Magic of Deep Learning

**This feels like magic.** I mean, it doesn't really seem like we could just:
- Start with nothing
- Start with random word vectors and a pile of text
- Say, "do some math"
- And we will get something useful out

But **the miracle of what happens in these deep learning spaces is we do get something useful out!**

---

## Gradient Descent and Optimization

The optimization process:
- Takes the **actual observed u vector**
- Compares it to the **weighted prediction**, the weighted sum of our current ux vectors, based on how likely they were to occur

What you'd like to have is your **expectation** (the weighted average) to be the same as what was observed, because then:
- You'll get a **derivative of 0**
- Which means that you've **hit a maximum**

This is the formula of **partial derivative with respect to center vector parameters**. You can also go with **outside vector parameters**.