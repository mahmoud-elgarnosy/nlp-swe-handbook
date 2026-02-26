# Recurrent Neural Networks: From Theory to Advanced Architectures

## Table of Contents

1. [Introduction to Sequential Data Models](#introduction-to-sequential-data-models)
2. [Variable Length Data Challenge](#variable-length-data-challenge)
3. [Recurrent Neural Networks (RNN) Fundamentals](#recurrent-neural-networks-rnn-fundamentals)
4. [Backpropagation Through Time](#backpropagation-through-time)
5. [Training Challenges in RNNs](#training-challenges-in-rnns)
6. [RNN as Generalization of HMM](#rnn-as-generalization-of-hmm)
7. [Bidirectional RNNs](#bidirectional-rnns)
8. [Encoder-Decoder Architecture](#encoder-decoder-architecture)
9. [Long Short-Term Memory (LSTM)](#long-short-term-memory-lstm)
10. [Gated Recurrent Units (GRU)](#gated-recurrent-units-gru)
11. [Attention Mechanism](#attention-mechanism)
12. [Recursive Neural Networks](#recursive-neural-networks)

---

## Introduction to Sequential Data Models

**Graph 1:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_e5c985fa-a3d3-40b6-9303-aa2503b95f17.jpeg)

[Timestamp: 00:17](https://youtu.be/lClNhXVNZ-0?t=17)

These are models that are suitable for **sequential data that doesn't have necessarily a fixed length**. We've already seen a Hidden Markov Model (HMM), and now we're going to see how:

- **Recurrent Neural Networks (RNN)** generalize those Hidden Markov Models
- **Recursive Neural Networks** generalize this even further

---

## Variable Length Data Challenge

**Graph 2:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_191d5dcc-b799-4412-9925-789b32e436ac.jpeg)

[Timestamp: 00:47](https://youtu.be/lClNhXVNZ-0?t=47)

### The Problem with Feed-Forward Networks

When it comes to variable length data, the issue is that all the neural networks we've talked about so far are **feed-forward NNs** that assume:

- The input is of a **fixed length**
- We have a vector of input with a fixed number of features (M measurements)
- The architecture works given this specific input size

### When Fixed Architectures Fail

When the input size changes, we encounter problems:

- **Time series data** with different sequence lengths
- **Machine translation** requiring flexible architectures
- We need a model that essentially adjusts with the sequence length → **Recurrent NN**

### Beyond Sequences

In some situations, it's not just that we have a sequence, but we'd like to perhaps generalize this to work with:

- **Trees**
- **Graphs**

This leads to **recursive neural networks**.

---

## Recurrent Neural Networks (RNN) Fundamentals

**Graph 3:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_b075ef75-0175-478a-9209-9cd51eef30f7.jpeg)

[Timestamp: 02:58](https://youtu.be/lClNhXVNZ-0?t=178)

### Core Concept

A **recurrent NN** is best understood as a neural network where:

- There are some **nodes that are recurrent**
- The **output** of that node is **fed back as input** to that node
- This creates a **cycle**
- The cycle doesn't have to be just with respect to one node—several nodes might be involved

### How It Works

The **black square** indicates an arc that is recurrent:

- At every time step, we get some input **x**
- It feeds into some hidden vector **h**
- This hidden vector has an output that is **fed back**
- This corresponds to a link from one time step to the next
- The next **h** depends on:
  - The previous **h**
  - The current **x**

---

## Backpropagation Through Time

**Graph 4:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_a85d1e23-8fca-42e8-adf2-47b654fda092.jpeg)

[Timestamp: 05:38](https://youtu.be/lClNhXVNZ-0?t=338)

### The Original Approach

**Backpropagation through time** was the original method for gradient descent in RNNs.

### The Unrolling Concept

When we say "backpropagation through time," it corresponds to:

- Having a recurrent NN that we **unroll over time**
- Working with an unrolled version to a certain depth
- Treating this as a **feed-forward NN**
- Simply propagating our gradient back through this network

> **Key Insight:** At some level, training a recurrent NN is not that much more difficult than training a regular feed-forward NN because we just unroll our network, effectively create a feed-forward NN, and then do backpropagation through this.

### The Critical Difference: Weight Sharing

**There is still one difference:**

Because of the recurrence, the way we compute **h** at every time step uses the **same function f**:

- This little **f** indicates we have a function that takes as input:
  - The previous **h**
  - The current **x**
- It produces the current **h**
- It's that **same function** applied at every time step

**If that function has some weights, it means we need to use the same weights at every time step, so there is a certain amount of weight sharing that occurs.**

---

## Training Challenges in RNNs

**Graph 5:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_4cf5e41d-5a3c-4c75-bf32-f1517d5858ce.jpeg)

[Timestamp: 09:06](https://youtu.be/lClNhXVNZ-0?t=546)

### Function Complexity

This could be a simple way of combining **h** and **x** to produce the next **h**:

- Using the **sigmoid activation function** as an example
- But you could have any activation function you want

More generally, **f** doesn't have to be just one simple function:

- It could be a **whole composition of functions**
- We could have an entire **deep NN** to compute what the next **h** is going to be
- Some architectures have a somewhat more complex way of computing **h** at every time step

### Weight Sharing Impact

You can see the function **f** repeats, which means:

- We have the **same weights** used
- Therefore, there's a certain amount of **weight sharing**
- When we do backpropagation, we must account for shared weights
- The derivation of the gradient (partial derivatives) will be a little different
- But with automatic differentiation packages, it will compute that for you

> **Note:** **h** is a **vector**, not a scalar.

### Major Training Challenges

What is challenging in terms of training:

#### 1. Gradient Vanishing Problem

- At every time step, we have partial derivatives where we multiply a bunch of factors
- If these factors are all **smaller than one**, the product gets smaller and smaller
- Eventually converges towards zero
- This is the **gradient vanishing problem**

#### 2. Gradient Explosion Problem

Since it's the **same function f** applied at every time step:

- We end up with some factors that are essentially the same at every time step
- If we have numbers **greater than one** and multiply them repeatedly
- The product keeps increasing exponentially with sequence length
- This is the **gradient explosion problem**

#### 3. Long-Range Memory Problem

Sometimes we need to account for memory over a long range:

- The beauty of RNN is handling sequences of arbitrary length
- But it's not clear it will remember information from early inputs
- At every step we compute some hidden state that gets modified repeatedly
- **The info from the first input might get lost gradually at every time step when we modify h**

**Application Context:**

- For **activity recognition** with sensor measurements: Natural forgetting of older measurements is okay
- For applications where past inputs remain important: This becomes an issue

#### 4. Prediction Drift

For predicting weather several steps into the future:

- Use RNN with input measurements from the past
- Unroll for a couple of time steps and make predictions (temperature, wind, rain, etc.)
- **Any error in prediction at each time step will build up**
- Predictions start **drifting** as we predict further into the future

---

## RNN as Generalization of HMM

**Graph 6:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_e1da8a2f-33a0-4ec6-83df-19edb2a00629.jpeg)

[Timestamp: 19:07](https://youtu.be/lClNhXVNZ-0?t=1147)

We can think of **RNN as a way of generalizing HMM**. Here's an RNN that allows us to represent an HMM.

**Graph 7:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_fc342657-b8fc-4df2-87c5-0e938fce0619.jpeg)

[Timestamp: 21:23](https://youtu.be/lClNhXVNZ-0?t=1283)

### Understanding Causal Relationships

That's why we have arcs that go towards **x**:

- In **activity recognition**: A person executes an action/activity (**y**)
- This triggers a measurement (**x**)
- We feed **x** into our system and try to compute **y**
- But in reality, the causal relationship is:
  - We execute some action/activity **y**
  - Which causes or triggers a measurement **x**
- **That's why the arrow goes this way**

**Graph 8:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_a6abd3c9-50d0-4bf1-97e7-ac1b3714b85b.jpeg)

[Timestamp: 26:14](https://youtu.be/lClNhXVNZ-0?t=1574)

### Output Computation

For some applications:

- The **output is the same as the hidden state**
- More generally, the output depends on the hidden state
- We transform it through some function **g** or **K**

**Example of function definitions shown in the graph.**

### Key Difference: Edges

- In **HMM**: Edges indicate **probabilistic dependences**
- In **RNN**: Edges indicate **functional dependencies**

### RNN Emulating HMM

One way of thinking about how an RNN can emulate an HMM in classification (e.g., activity recognition):

**In HMM**, the forward algorithm computes:

- y₁|x₁
- y₂|x₁,x₂
- y₃|x₁,x₂,x₃
- y₄|x₁,x₂,x₃,x₄

**Graph 9:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_892ef9a3-a029-447a-8621-0f951958e285.jpeg)

[Timestamp: 27:46](https://youtu.be/lClNhXVNZ-0?t=1666)

**In RNN**, looking at functional dependencies, we can compute:

- y₁ based on x₁
- y₂ based on x₁, x₂
- y₃ based on x₁, x₂, x₃
- y₄ based on x₁, x₂, x₃, x₄

So we can go forward in the same way.

---

## Bidirectional RNNs

**Graph 10:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_75ee3bc7-842c-4412-a3ec-bcad3cf8df9b.jpeg)

[Timestamp: 27:42](https://youtu.be/lClNhXVNZ-0?t=1662)

**Graph 11:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_8e047b1a-b1fb-49fc-8cb7-d23944975d27.jpeg)

[Timestamp: 28:06](https://youtu.be/lClNhXVNZ-0?t=1686)

### The Need for Backward Processing

In Hidden Markov Models, we talked about several tasks, including **hindsight**:

- **Forward phase**: Aggregating inputs up to that time step
- **Backward phase**: Accumulating information from measurements that occurred afterward

### Application: Speech Recognition

This is very important for applications like **delayed speech recognition**:

- The system has an entire sentence that is spoken
- Then processes the entire signal to predict the words at every time step
- A word in the middle of the sentence can be recognized based on:
  - The audio signal **up to that time**
  - The audio signal **afterward**

### The Problem with Standard RNNs

For RNN, the architecture with arrows only pointing forward makes it impossible to:

- Have x₃ influence the prediction for y₂
- Have x₄ influence the prediction for y₂
- The arrows are only going forward

**But for some applications, we need backward computation as well.**

**Graph 12:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_0478a2fb-6304-4fbb-b4ba-2101342f1d1c.jpeg)

[Timestamp: 33:15](https://youtu.be/lClNhXVNZ-0?t=1995)

### How Bidirectional RNNs Work

Let's say we need to calculate **y₂**, and we want it to depend on x₁, x₂, x₃, x₄:

- **y₂** depends on:
  - **Forward h₂**
  - **Backward h₃**
- **Forward h₂** depends on:
  - x₁, x₂, h₁
- **Backward h₃** depends on:
  - x₃, x₄, h₄

**Graph 13:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_6461d5b6-5893-4c4f-9791-404a103082a4.jpeg)

[Timestamp: 34:03](https://youtu.be/lClNhXVNZ-0?t=2043)

---

## Encoder-Decoder Architecture

**The most important architecture you will see more than once: The Encoder-Decoder Model**

**Graph 14:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_d3b499da-f49d-488a-a054-6bd9ddd335a3.jpeg)

[Timestamp: 35:22](https://youtu.be/lClNhXVNZ-0?t=2122)

### The Challenge

Some applications have corresponding output for every input:

- Activity recognition
- Speech tagging

But many other applications have sequences where **input and output sequences don't match naturally**:

- **Machine translation**: 4 words in one language → 5 or 3 words in another
- Sentences may have **word ordering changed**
- RNNs naturally assume synchronization between inputs and outputs

### Applications Requiring Different Lengths

- **Machine translation**
- **Question answering**
- **Conversational agents** (dialogue)

### The Solution: Encoder-Decoder Model

Also known as the **Sequence-to-Sequence model**.

#### The Encoder

One approach uses a first RNN as an **encoder**:

- Purpose: **Encode or summarize the input**
- For machine translation: Read a sentence and summarize it
- Gets summarized into a vector **C** (the **context vector**)
- This vector is some sort of **embedding of the input**

#### The Decoder

Once we've summarized the input into context vector **C**:

- We produce the output sequence
- For machine translation: The corresponding translation
- For question answering: The answer
- For dialogue: The response

A **second RNN** takes the context vector as input and produces outputs.

### The Analogy

It's like:

1. You're given a page with a sentence
2. You read it
3. You put the page away
4. You **remember the sentence in your head** (because you've read it)
5. Now you start writing the translation
6. **C is our memory** where we remember what we've read

Based on that memory:

- We produce the first word of translation
- As we produce the translation, we write words one by one
- It's useful to **keep track of progress** as we write

There are **hidden states** that:

- Keep track of the progress of translation
- Summarize what we've translated so far
- Help determine what the next word should be
- Keep track of what we've produced so far
- Produce words that are coherent with each other

### Why Feed C at Every Step?

We feed the context vector into **y** at every step because:

- **y** is a hidden state that captures what's relevant
- At any point, we should remember **what we're trying to translate**
- The context vector is our **summary of the sentence**
- We need to remember this as we produce our translation
- We also need to remember **what we've already translated**

Both **C** and the previous hidden state (corresponding to already output words) are used to determine the next word.

### The Role of C Throughout

**C** is used to:

- Inform the hidden state
- Help produce the **y**'s

We could argue that if our architecture is well-designed, we might just need an arrow from **C** to the first hidden vector. But remember the **information forgetting problem**:

- If we only feed **C** as input at the beginning
- We might know what we're translating initially
- But at every time step when we modify the hidden state
- We might **gradually forget about C**

**Solution:** It's useful to be able to "peek back"—like checking your memory periodically to not forget what you're translating. **These arcs allow the RNN to remember and not forget.**

> **Note:** **C** is a large embedding vector, typically with **500 to 1000 values**.

**Graph 15:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_eeac9f18-6bcb-4736-88bc-6bb6375638d9.jpeg)

[Timestamp: 46:14](https://youtu.be/lClNhXVNZ-0?t=2774)

### The 2014 Revolution

Machine translation was **transformed in 2014**:

- **Previously**: Team of researchers, several years, complete pipeline with multiple techniques (probabilistic models, HMM)
- **After RNN**: One researcher, one desktop, **one week**
- The process that used to take **3 years** was dramatically simplified

---

## Long Short-Term Memory (LSTM)

**Graph 16:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_97bd5e0d-1891-4d20-a2bc-5a9beb4c4090.jpeg)

[Timestamp: 49:49](https://youtu.be/lClNhXVNZ-0?t=2989)

### The Problems with Standard RNNs

1. **Gradient vanishing problem**
2. **Gradient explosion problem**
3. **Long-range dependencies**

We need to solve or at least mitigate these problems.

### The Key Innovation: Gates

The key to **LSTM** is using **gates**:

- **Input gate**
- **Forget gate**
- **Output gate**

These three gates regulate:

- What happens to the input
- What happens to the hidden state
- What happens to the output

### How Gates Work

Whenever there's a gate:

- The gate takes a value **between 0 and 1**
- Produced by a **sigmoid activation function**
- **If gate = 0**: Gate is **closed**
  - Multiplying by 0 nullifies the input
  - Prevents input from going into the cell state
- **If gate = 1**: Gate is **open**
  - Allows information to flow through

### The Three Gates

**Input Gate:**

- Responsible for nullifying **x** or not
- Controls whether new input enters the cell

**Output Gate:**

- Responsible for letting current **h** pass to the function that calculates the output or not
- Controls what information leaves the cell

**Forget Gate:**

- Responsible for letting current **h** pass to the function that calculates the next **h** or not
- Controls what information is retained in memory

**All of those gates take as input:**

- The **x** at the current step
- The **h** at the previous step
- They decide together whether information should flow through or not

**Graph 17:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_d0aa42b1-7a60-4405-9827-1697e383f16c.jpeg)

[Timestamp: 56:22](https://youtu.be/lClNhXVNZ-0?t=3382)

**Graph 18:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_d0aa42b1-7a60-4405-9827-1697e383f16c.jpeg)

[Timestamp: 56:53](https://youtu.be/lClNhXVNZ-0?t=3413)

**Graph 19:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_c47eeb32-4c2d-48b6-b760-28464697b8bb.jpeg)

[Timestamp: 57:40](https://youtu.be/lClNhXVNZ-0?t=3460)

### The Practical Variant

**Graph 20:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_ee3ee077-e0cb-4ec2-b60e-533458dbed7e.jpeg)

[Timestamp: 58:46](https://youtu.be/lClNhXVNZ-0?t=3526)

In the variant used in practice:

- The gates receive a **contribution from the current x**
- Plus a **contribution from the previous output**

### Why LSTM Solves the Problems

#### Preventing Gradient Vanishing and Explosion

The introduction of gates is useful because:

- At every step when we compute the cell state
- Gates determine what can influence the cell state
- Gates make sure the output doesn't grow or shrink arbitrarily
- The **tanh function** produces values between **-1 and 1**
- This prevents output from growing arbitrarily
- Has similar impact on gradients
- Gradients don't shrink arbitrarily low and don't explode

#### Long-Range Memory

The benefit of gates for memory:

- They **regulate more explicitly what stays in memory**
- The **cell state** is propagated from one time step to another
- This is what **carries the memory** of the network
- Gates determine if we forget what was in memory or not

**Memory Persistence:**

- If we **don't want to forget**: Set forget gate to **1**
  - Previous memory flows through
- If we **don't want to perturb** previous memory: Set input gate to **0**
  - No contribution from the input
  - Cell state persists without being changed

**The intuition:** With those gates, we have an **explicit mechanism** that the network can optimize to determine:

- What should remain in memory
- What should be added to memory
- What should be output from memory
- This makes it easier to have **persistence for longer-range dependencies**

---

## Gated Recurrent Units (GRU)

**Graph 21:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_d4450903-1a0e-486d-a107-a8d4b5c351bf.jpeg)

[Timestamp: 01:04:50](https://youtu.be/lClNhXVNZ-0?t=3890)

### Simplification of LSTM

**GRU** is a simpler alternative to LSTM.

### The Update Gate

The **update gate** decides:

- Whether the new input goes into the hidden state
- Or we simply preserve what we already had in the hidden state

**The idea:**

- When we allow new input into the hidden state, it perturbs what we already had
- Instead of having two different gates for this
- We use a **single gate** that takes a **convex combination** of:
  - The input
  - The previous hidden state

### LSTM vs GRU

The choice between **LSTM** or **GRU** is **problem-dependent**.

---

## Attention Mechanism

**The most important concept for NLP and even Computer Vision**

**Graph 22:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_2d000fbe-3271-4d3e-950c-9ea4754bba87.jpeg)

[Timestamp: 01:07:33](https://youtu.be/lClNhXVNZ-0?t=4053)

### The Remaining Challenge

Even though we can deal with longer-range dependencies using LSTM and GRU through their gating mechanism:

- The reality is that some memory might still get **perturbed at every step**
- Whenever we include something from the input, we are **perturbing what we have in memory**
- There doesn't seem to be a way to really account for everything if we want to both:
  - Preserve what was in memory
  - Add new input

### The Solution: Attention

One solution is to create another architecture known as **attention**.

### The Intuition

When doing translation:

1. You've read your input sentence
2. You've stored it in memory
3. Now you start decoding by producing the translation
4. **When you start decoding, it would be helpful if you could go back and check what were the words of the original sentence**

Even if you have the meaning in your head:

- **Simply looking back at the original words helps**
- In machine translation, you can often do fairly well with **word-for-word translation**
- If you're about to produce a word, it's good to make sure this word corresponds to some previous word in the original sentence

**An attention mechanism:**

- Tries to **align every word in the output sequence with some words in the input sequence**
- But **not necessarily in order**—might align them in a different order
- Different languages have words that come in different order

### Dynamic Context Vectors

The idea:

- Instead of having **one context vector** summarizing everything we've read
- Compute a **context vector at every time step** of decoding
- Make that context vector **more dependent on specific words** of the input sentence
- Rather than the entire input sentence

**Translation analogy:**

- If translating an entire document, you don't memorize it all then start translating
- Instead, you **start producing the translation**
- Then **go back every once in a while** to see what words/sentences come next
- Do the translation **gradually**

**Result:** We want a context vector that:

- Summarizes the context
- Or whatever we want to produce next
- Doesn't have to summarize the entire sentence
- Just the **stuff that matters for the next few words**

### Computing Alignment

The **softmax** produces a **higher probability** whenever we have an alignment between:

- What we're trying to output
- Some input words

**Alignment** can be as simple as computing the **dot product**.

**Graph 23:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_2d000fbe-3271-4d3e-950c-9ea4754bba87.jpeg)

[Timestamp: 01:12:24](https://youtu.be/lClNhXVNZ-0?t=4344)

### How Attention Typically Works

**Three Steps:**

1. **Compute alignment through dot product**
   - See how well output should align with each input
2. **Compute alignment weights through softmax**
   - Turn alignments into probabilities
3. **Compute context vector through convex combination**
   - Weighted combination of alignment weights and corresponding input **h**'s

We're going to have a **linear combination** (actually a **convex combination**):

- Trying to align something with a bunch of possible inputs
- Compute some weighted combination of those inputs
- Compute how well the output should align with each input
- This alignment can be computed by a **dot product**
- Then turned into a probability through a **softmax**

**Graph 24:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_0379fabc-9650-401c-946b-0e1a88b7eaed.jpeg)

[Timestamp: 01:16:50](https://youtu.be/lClNhXVNZ-0?t=4610)

### Attention in Practice

As before, we're computing some hidden state at every time step.

**Creating the context vector:**

- Instead of taking the last **h** as the context vector
- We take a **linear combination** (convex combination) of those hidden states
- This way we can align better with the hidden state that corresponds to the better input for translation

**Example:**

- When producing the third word
- We're at a point where we're about to translate a certain concept
- That concept is somewhere in the input—but **where**?
- We compute some **alignment** to find it

**The weights (A's):**

- Are **probabilities** from a softmax
- Used to take a convex combination of hidden vectors
- If one hidden vector has a **higher weight**:
  - It produces a **better context** than the other hidden vectors
  - That hidden vector captures better the concept

we're interested in for translation at this point

**So the intuition:** The **a's** are probabilities from a softmax, obtained by computing alignment between the previous **s** and all the **x's**, to determine what the next **s** should be.

**Graph 25:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_7e96d7e5-522a-4cd6-8ab7-c4c3c5bbf032.jpeg)

[Timestamp: 01:19:40](https://youtu.be/lClNhXVNZ-0?t=4780)

### Performance Benefits

**RNN with attention achieves the highest BLEU score for all sentence lengths.** This difference appears clearly for long sentences.

**Why it works:**

- **All words** up to the current step have influence on the current output
- Early words are **not necessarily less important** than later words
- They all have a chance to influence the output
- **How far in the past** has **no impact**
- It's like: read part of the sentence, translate it, keep reading, keep translating

**RNN without attention:**

- Degrades clearly with long sequence length
- Even **LSTM and GRU** are not completely effective

**Graph 26:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_6b724b46-c66a-4717-a6b1-df7f4d90f34c.jpeg)

[Timestamp: 01:27:04](https://youtu.be/lClNhXVNZ-0?t=5224)

### Language Differences

Very often with different languages:

- You **don't have word-for-word alignment**
- The **number of words might differ**
- The **ordering of words might differ**

---

## Recursive Neural Networks

**Graph 27:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_d5aaf065-a54c-45af-8277-f0282a1805d1.jpeg)

[Timestamp: 01:29:43](https://youtu.be/lClNhXVNZ-0?t=5383)

### When to Use Recursive NNs

**Recursive NN** is good in situations where we'd like to treat our data as:

- Not so much corresponding to a **sequence**
- But corresponding to a **graph** or a **tree**
- **Combine pairs recursively**

**Graph 28:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_a6aedce3-cf2d-4ac9-86ae-14b99f130398.jpeg)

[Timestamp: 01:34:22](https://youtu.be/lClNhXVNZ-0?t=5662)

**Graph 29:** ![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_67c513b6-d43b-4887-971f-646dc077565f.jpeg)

[Timestamp: 01:36:40](https://youtu.be/lClNhXVNZ-0?t=5800)

### Application

A **useful way to come up with the embedding of a sentence** using recursive neural networks.

---

**End of Document**