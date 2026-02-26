  

﻿[00:17](https://youtu.be/lClNhXVNZ-0?t=17)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_e5c985fa-a3d3-40b6-9303-aa2503b95f17.jpeg)

these are models that are suitable for sequential data that doesn't have necessarily a fixed length, so we've already seen a hidden Markov model, now we're going to see how recurrent NN is generalizing those hidden Markov models, and then the recursive NN generalized this even further

﻿[00:47](https://youtu.be/lClNhXVNZ-0?t=47)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_191d5dcc-b799-4412-9925-789b32e436ac.jpeg)

  

so when it comes to variable length data the issue is that all the NN that we've talked about so far that our feed-forward NNs they assume that the input is of a fixed length, so we've a vector of input, so the x's are there's a fixed number of them, and then that corresponds to let's say M different measurements, or M different features, we feed that in, and then we have an architecture that works given this input size, but now if the input size changes, so as a concrete example, let's say that we've got a some time series data, so we have different lengths of sequences then we can't have a fixed architecture that would necessarily work with arbitrary length, machine translation is a good example for this, we want a model will essentially adjust with the length of the sequence as a Recurrent NN

in some situations, it's not just that we have a sequence, but we'd like to perhaps generalize this to work with trees or even graphs, and this will lead as well to recursive NN

  

  

  

﻿[02:58](https://youtu.be/lClNhXVNZ-0?t=178)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_b075ef75-0175-478a-9209-9cd51eef30f7.jpeg)

so a recurrent NN, the best way to think about it is that it is an NN where there are some nodes that are recurrent in the sense that the output of that node will be fed back as input to that node, so this creates a cycle, and now it doesn't have to be just with respect to one node there might be several nodes involved into this cycle, but that's the main idea

  

the black square here indicates an arc that is recurrent, the idea is that at every time step, I would get some input x, it would feed into some hidden vector h, and then this hidden vector would, now have an output that is feedback, so that corresponds to essentially a link from one-time step to the next, and then in this way the next h, depends on the previous h, but also depends on x

  

  

﻿[05:38](https://youtu.be/lClNhXVNZ-0?t=338)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_a85d1e23-8fca-42e8-adf2-47b654fda092.jpeg)

  

backpropagation through time was the originally for gradient descent, now when we say backpropagation through time, it corresponds to the idea that we have a recurrent NN that we unroll over time, when we do the gradient descent what we effectively do is gradient descent with respect to the unrolled version of the network

  

we are really working with an unrolled version to a certain depth up to a certain length, and now we can simply treat this as a feed-forward NN, and simply propagate our gradient back through this network, so you see at some level training a recurrent NN is not that much more difficult than training a regular feed-forward NN because we just unroll our Network effectively create a feed-forward NN, and then just do backpropagation through this

**Now there is still one difference, the difference is that when you look at this network, because of the recurrence, the way we compute h, at every time step is going to be using the same function f, so here this little f indicated that we have a function that takes as input the previous h, and the current x produces their current h, and then it's that same function, that is applied at every time step now if that function has some weights, it means that what we need to use the same ways as well at every time step, so there is a certain amount of weight sharing that occurs**

  

﻿[09:06](https://youtu.be/lClNhXVNZ-0?t=546)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_4cf5e41d-5a3c-4c75-bf32-f1517d5858ce.jpeg)

  

so this could be a simple way of combining h, and x to produce, the next h, and then here I'm just giving as an example the sigmoid activation function, but you could have any activation function you want, now more generally F doesn't have to be just one simple function like this it could be a whole composition of functions, so we could imagine having an entire deep NN, just to compute what the next h is going to be, and we're going to be and we're going to see in a moment some architectures that indeed have a somewhat more complex way of computing what the h is at every time step,

  

now you can see the function f, that repeats, and then that means we have the same weights that are used so therefore, there's some certain amount of weight sharing that takes place, so when we do our backpropagation, and we have to take into account the fact that we have some shared weights, what it means is that the derivation of the gradient, the partial derivatives is going to be a little different because of that but otherwise let's say you use a package with automatic differentiation it will compute that for you, and the fact that the weights are shared is not a big deal, so this can be done without any problem

  

note: h is a vector not a scaler

  

what is challenging in terms of the training is that now we have again, the gradient vanishing problem, and more than that, we can also have a gradient explosion problem

the intuition here when we talked about that was that at every time step in every day year we would have partial derivatives where we multiply a bunch of factors, and then if these factors are all smaller than one whenever you multiply numbers that are smaller than one, the product just gets smaller, and smaller and smaller, and eventually you know converges towards zero all, so this was the gradient vanishing problem

Now what happens in RNN that's the same function f that is applied at every time step, so when you do the computation of the gradient, you're going to end up with some factors that are going to be essentially the same also at every time step, so now clearly if we have numbers that are smaller than one is going to vanish, but now if we have numbers that are greater than one, we're going to have the opposite problem, if you multiply factors that are greater than one, then you product keeps on increasing and in fact will increase exponentially in the length of the sequence and that's known as a gradient explosion problem

  

another issue is sometimes we would like to be able to take into account memory over a long range, so the beauty of RNN is that it can handle sequences of data of arbitrary length, but it is not clear, that it will necessarily remember the information from the early inputs, so you see at every step we compute some hidden state and then it gets modified and modified and modified, **but now the info from first input that goes into h, might get lost gradually, and every time step when we modify h**

for some applications where how far into the past an input was should still be as important then that's gonna be an issue, so in other applications like see activity recognition that we talked about where we have some sensor measurements there it kind of okay if we forget about the sensor measurements that happen a while ago, because there is natural importance about sensor measurements that are more recent, so then a RNN will have this natural forgetting, which is okay in that context

  

prediction drift: let's say I want to predict weather several steps into the future, we can use RNN and take input measurements from the past, and then it will unroll for a couple of times steps and them make a prediction, about the temperature maybe the wind and amount of rain, and so on, let's see a few days into the future, so here if you apply the recurrence, every time step would correspond to a different date then naturally what's going to happen is that any sort of error into the prediction at each time step will build up as we start predicting further into the future, and then our prediction are gonna start drifting

  

  

﻿[19:07](https://youtu.be/lClNhXVNZ-0?t=1147)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_e1da8a2f-33a0-4ec6-83df-19edb2a00629.jpeg)

now we can think of RNN as a way of generalizing HMM and for this I"'m gonna draw an RNN that does indeed allow us to represent an HMM

  

﻿[21:23](https://youtu.be/lClNhXVNZ-0?t=1283)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_fc342657-b8fc-4df2-87c5-0e938fce0619.jpeg)

so that's why, we have arcs that go towards x, so in the case of activity recognition, you see a person execute an action, an activity that corresponds to y, and then that triggers a measurement which is x, now we feed into our system x, and then we're trying to compute y, but in reality in nature, what is the causal relationship is that we essentially execute some action or some activity y, which causes or triggers a measurement x, and that's why the arrow goes this way

  

﻿[26:14](https://youtu.be/lClNhXVNZ-0?t=1574)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_a6abd3c9-50d0-4bf1-97e7-ac1b3714b85b.jpeg)

  

  

for some applications, the output is the same as the hidden state, but more generally, we can just say that the output will just depend on the hidden state and perhaps we just need to transform it through some function g K

  

This is one example of how these functions could be defined

  

remember that in HMM edges indicated probabilistic dependences, while in RNN edges indicated functional dependencies

  

Now one way of thinking about how an RNN can be used to essentially emulate an HMM, in the context of classifications, so activity recognition what we did is we have the forward algorithm that would compute y1|x1, y2|x1,x2, y3|x1,x2,x3, y4|x1,x2,x3,x4

  

﻿[27:46](https://youtu.be/lClNhXVNZ-0?t=1666)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_892ef9a3-a029-447a-8621-0f951958e285.jpeg)

  

  

and in the same way of RNN, if we look at the functional dependencies, we can compute y1 based on x1 then we can compute y2 based on x1 and x2, and y3 =====>x1,x2,x3,

y4=====>x1,x2,x3,x4, so we can go forward in the same way

  

﻿[27:42](https://youtu.be/lClNhXVNZ-0?t=1662)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_75ee3bc7-842c-4412-a3ec-bcad3cf8df9b.jpeg)

  

  

﻿[28:06](https://youtu.be/lClNhXVNZ-0?t=1686)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_8e047b1a-b1fb-49fc-8cb7-d23944975d27.jpeg)

  

so in hidden Markov models, we talked about several tasks, one of them was hindsight, in that what we needed to do was essentially compute in a forward way, what some aggregation of our inputs that occurred up to that time step, and then also there was a backward phase, where we accumulating information measurements that occurred afterward so in some applications, like speech recognition, this is very important because often you do delayed speech recognition where you have essentially an entire sentence that is spoken and then at that point the system starts processing the entire signal to predict or to recover what were the words that were set at every time step, so in the middle of the sentence, there's a word, and then that word , you can recognize it based on the audio signal up to that time, and also including the audio signal afterward, so when we do this with an HMM, it's not a problem, we saw that there are ways of doing the computation. so that the information can flow in a forward way up to the point where we want to recognize a hidden state and then we have a backward phase that also brings information back in time up to that same hidden state, but now for RNN, the architecture the way the arrows are pointing it's not possible to have x3 influence my prediction for y2 or to have x4 influence my prediction for y2, because the arrows are only going forward, but for some application like those we need to be able also for doing backward computation

  

﻿[33:15](https://youtu.be/lClNhXVNZ-0?t=1995)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_0478a2fb-6304-4fbb-b4ba-2101342f1d1c.jpeg)

  

let's say we need to calculate y2, and I would like y2 to depend on x1, x2, x3, x4, y2 depends on the forward h2 and the backward h3, and if you look at the forward h2, it depends on x1, x2, h1, and if you look at the backward h3, it depends on this block x3, x4, h4

  

﻿[34:03](https://youtu.be/lClNhXVNZ-0?t=2043)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_6461d5b6-5893-4c4f-9791-404a103082a4.jpeg)

  

  

  

the most important arch that you will see more than once time, the Encoder-Decoder Model

﻿[35:22](https://youtu.be/lClNhXVNZ-0?t=2122)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_d3b499da-f49d-488a-a054-6bd9ddd335a3.jpeg)

  

there are some applications that there's corresponding output for every input, like activity recognition and also speech tagging, bu then there are a lot of other applications where there are some sequences, but the input sequence and the output sequence don't match naturally, so like in machine translation, I might have a sentence in one language let's see it has four words, and then in the next language when we're translating the output might be of a different length, there might be five words or there might be three words, so sometimes those sentences they have their word ordering changed, so here RNN naturally assume a certain amount of synchronization between the inputs and the outputs,

  

so if you want to tackle problems like machine translation, question answering, and conversational agents that produce a dialogue, you can also use RNN to deal with the input and output of different lengths, but perhaps with a different arch, so a common arch for this is known as the Encoder-Decoder model which is also known as the sequence to sequence model

  

one approach is just to say that I'm gonna have a first RNN that corresponds to an encoder, the purpose of the encoder will be to encode or if you prefer to summarize the input, so if I'm doing machine translation, I've got a sentence as input and then my RNN is going to read it and then summarize it, and then it's gonna get summarized into a vector C, this vector is known as the context vector, it's some sort of embedding of the input

So once we've summarized the input into this context vector c, then we'll want to produce the output sequence so in the case of machine translation that's the corresponding translation, for question-answering that's the answer, and for dialogue, this could be the response, so what we do is we have a second RNN that takes the context vector as input and then, we'll produce some outputs here

  

encode input to context vector, and then decode this context vector, -meaning vector-- to another sequence as output, it's a bit like, if you're given a page there's a sentence return you read it and now you put the page away, you remember the sentence in your head, because you've read it and now you start writing down what is the translation in the other language, but you remember in your head what is the meaning of that input sentence, so c is our memory where we remember what we've read so far, so base on that memory, now we produce the first word of that translation, but now as we produce the translation, we write the words one by one, and here it's actually useful to keep track of how we're progressing as we write down those words, so there's going to be some more hidden states, so these nodes here are hidden states that essentially keep track of the progress of the translation, so I produce a first word now, I include this first word into my hidden state to summarize essentially, what I've translated so far, so then I know what should be the next word and so on, this way I can keep track of what I've produced so far, and then I can gradually produce more words, that are coherent with each other

  

I'm feeling the context vector into y at every step because here we can think of y as a hidden state that captures what's relevant, so at any point in time I should remember, what it is that I'm trying to translate, so the context vector was my summary of the sentence, so I need to remember, that as I produce my translation and also need to remember what I've already translated, so that's why I feed both C and the previous hidden state that corresponds to my words, that I've output it already and then all of that information is then used to determine what should be the next word

so here c is used to inform the hidden state and is also used in the production of the y's, so I guess here we could argue at some level that perhaps if our architecture is well designed if we have the right function, we would just need an arrow from c to the first hidden vector, then I don't need all these arrows to all other hidden state or to y's, but then on our earlier discussion about information being forgotten over time, we've got the problem, that if all we do, is feed C as input here, then perhaps we'll know what we're translating at that time--first hidden state--but at every time step when we modify this hidden state, we might gradually forget about c, so it's useful to be able to sort of like peek back, it's a bit like you see, if you're read a sentence and now you've got a memory of it, but now every once in a while you go back an check your memory, so that you don't forget what it is that you're trying to translate, so that's what these arcs are doing essentially allowing the RNN to remember and not forget, because it might forget overtime

  

C is a large embedding vector, from five hundred or thousand values

  

﻿[46:14](https://youtu.be/lClNhXVNZ-0?t=2774)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_eeac9f18-6bcb-4736-88bc-6bb6375638d9.jpeg)

machine translation transformed in 2014 from a team of researchers that took several years to use several techniques--complete pipeline-- like probabilistic model HMM, to RNN, and one researcher with desktop and one week, while in the past the process used to take 3 years

  

  

﻿[49:49](https://youtu.be/lClNhXVNZ-0?t=2989)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_97bd5e0d-1891-4d20-a2bc-5a9beb4c4090.jpeg)

  

now what the problem with RNN is

  

1.  gradient vanishing problem
2.  gradient explosion problem
3.  long-range dependacies

now we need to solve these problems or at least mitigate them

the key to LSTM is using gates, we've got an input gate a forget gate, and an output gate, so these are three gates that essentially will regulate what happens to the input what happens to the hidden state, and what happens to the output in the sense that we whenever there's a gate this gate will take a value between 0 and 1, if it is 0, it means that the gate is closed, and then whenever we multiply the input gate--it's a number between 0 and 1 that's produced by a sigmoid activation function-- if it's closed, then it would be a value zero, and if I multiply by 0 it means that my inputs get nullified to zero, so it's the same as saying that we prevent the input from going into the cell state of the LSTM,

  

**in-gate**: responsible for nullifying x or not

**out-gate**: responsible for letting current h pass to the function that calculates the output or not

**forget-gate**: responsible for letting current h pass to the function that calculates the next h or not

  

**All of those gates are going to take as input the x at the current step and also the h at the previous step, so they decide together whether we should input to gate flow through or not**

the unrolled version of LSTM

  

﻿[56:22](https://youtu.be/lClNhXVNZ-0?t=3382)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_d0aa42b1-7a60-4405-9827-1697e383f16c.jpeg)

  

﻿[56:53](https://youtu.be/lClNhXVNZ-0?t=3413)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_9e47c5a3-b6b0-492c-9d62-ccb0483cb04a.jpeg)

  

﻿[57:40](https://youtu.be/lClNhXVNZ-0?t=3460)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_c47eeb32-4c2d-48b6-b760-28464697b8bb.jpeg)

  

  

Now the variant that is used in practice

  

﻿[58:46](https://youtu.be/lClNhXVNZ-0?t=3526)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_ee3ee077-e0cb-4ec2-b60e-533458dbed7e.jpeg)

here the change that the gates is a contribution from the current x and the previous output

  

  

one thing to realize about LSTM units is that the introduction of gates is useful to prevent the gradient vanishing, and explosion problem, because you see at every step when we compute the cell state, there are some gates that essentially determine what can influence the cell state, so as a result those gates can essentially make sure that the output doesn't grow, the output doesn't shrink, and then we also use the tanh function where this function is going to produce something that is necessarily between -1 and 1, so it does prevent the output from really just growing arbitrarily, and so this will have some impact as well in terms of the gradient and then for the gradient, there's some similar phenomenon where as a result we tend to have gradients that do not shrink and arbitrarily low and don't explode eighter

  

and in terms of long rang memory, the benefit of those gates is that they have to regulate more explicitly what stays in the memory, so here the cell state is being propagated from one-time step to another, that's what carriers the memory of the network, and this cell state, you see there's some gates that will determine if we forget what was already in the memory or not, so if we don't want to forget, then we have forget gate there as long as it's set to one it would mean here that the previous memory just flow through, and then if we don't want to perturb that previous memory then here the input gate could be set to zero, and as a result, there will be no contribution from the input, so we'll simply have a cell state that persist without being changed

  

**again the intuition with those gates we have an explicit mechanism that the network can use to optimize what should remain in the memory, and what should be added to the memory, and what the output from the memory, and then it makes it easier to essentially have some persistence for a longer range dependencies**

  

  

﻿[01:04:50](https://youtu.be/lClNhXVNZ-0?t=3890)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_d4450903-1a0e-486d-a107-a8d4b5c351bf.jpeg)

update gate decides whether the new input goes into the hidden state or we simply preserve what we already had in the hidden state, the idea is that you see, when I allow the new input to into the hidden state obviously perturbing what were already had in the hidden state, instead of having two different gates that are doing different things for this, we might as well have a single gate that will take a convex combination of the input and the previous hidden state

  

LSTM or GRU: is already problem-dependent

  

  

The most important thing, to complete in NLP or even Computer Vision

﻿[01:07:33](https://youtu.be/lClNhXVNZ-0?t=4053)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_2d000fbe-3271-4d3e-950c-9ea4754bba87.jpeg)

the next important thing regarding I guess sequence modeling is that even though, we can deal with longer-range dependencies using the LSTM unit and GRU unit through their gating mechanism, the reality is still that some memory might get perturbed at every step and then really like whenever we include something from the input we are perturbing what we have in the memory, so like there doesn't seem to be a way to really account for everything if we want to both preserve what was in the memory and add to this some new input, one solution to this is to create another architecture known as attention

  

the intuition is that you do your translation, you've read your input sentence, you've started in memory, and now you start decoding by producing the translation, but then when you start the decoding process it would be helpful if you could go back, and check again what were the words of the original sentence because even if you have the original sentence in your head at least you have the meaning of it in your head, **something just looking back at what were the original helps and then simply because in machine translation very often you can do fairly well just by doing word for word translation, so if you're about to produce a word, it might be good at make sure that this word indeed does correspond to some previous word in the original sentence so here if we want disability to go back and check what were those words perhaps we can use an attention mechanism they will simply try to align every word, in the output sequence with some words in the input sequence but not necessarily align them in order it might align them in a different order because different languages have words that come in different order**

  

so here the idea is that we have a context vector that we compute, but now instead of having a context vector that's just a summary of everything we've read so far we could simply try to come up with a context vector at every time step of out decoding and make that context vector more dependent on specific words of the input sentence as opposed to the entire input sentence because you see if you're translating let's see an entire document memorize them, and then start translating, that's crazy, instead **what we would do is more start producing the translation but then go back every once in a while and then see what are the words that come next or the sentences that come next into the original language, so that we can do the translation gradually, and then as a result we want a context vector that essentially will summarize the context or whatever it is that we want to produce next, it doesn't have to summarize the entire sentence but just this the stuff that matters for the next few words that I'm going to produce**

  

the idea is that softmax is going to essentially produce a higher probability whenever I've got an alignment between what I'm trying to output, and then some input words, alignment it could be as simple as just computing the dot product

  

  

﻿[01:12:24](https://youtu.be/lClNhXVNZ-0?t=4344)﻿

Attention typically works we're going to have a linear combination in fact a convex combination so we're trying to align something with a bunch of possible inputs we need to compute some weighted combination of those inputs and then for this, we're going to see how well the output should align with each one of the inputs and this alignment can be computed by a do product, and then turn into a probability through a softmax

  

1.  compute alignment through dot product
2.  compute alignment weight through softmax
3.  compute context vector through convex optimization of those alignment wights and corresponding input h's

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_2d000fbe-3271-4d3e-950c-9ea4754bba87.jpeg)

  

  

﻿[01:16:50](https://youtu.be/lClNhXVNZ-0?t=4610)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_0379fabc-9650-401c-946b-0e1a88b7eaed.jpeg)

as before we're computing some hidden state at every time step now when we want to create a context vector instead of taking the last h, and saying this is my context vector, why don't we instead take a linear combination in fact a convex combination of those hidden states, and then this way we can perhaps align better with the hidden state that would correspond to the the better input for the translation, so when I'm trying to produce the third word perhaps, I'm at a point in time, I'm about to translate a certain concept and now that concept is somewhere in my input but where is it this is where I'm going to have to compute some alignment so these weights, these A's are probabilities that come from a softmax and then I'm going to use them to take aconvex combination of my hidden vectors here such that if one hidden vector has a higher weight it means that it produces a better context than the other hidden vector, and that hidden vector might just capture better, the concepts, that I'm interested in for the translation at this point in time

  

**So if I go back to this line, the a's are the probabilities that would correspond to a softmax, and then these are obtained simply by computing some alignmen,t so I would compute an alignment between let's say the previous s and then all of the x's, so that I can compute, what the next s should be, so that the intutioun**

  

  

﻿[01:19:40](https://youtu.be/lClNhXVNZ-0?t=4780)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_7e96d7e5-522a-4cd6-8ab7-c4c3c5bbf032.jpeg)

RNN with attention achieves the highest Belu for all sentence lengths, this difference appears clearly for long sentences,

all words to the current step have influence on what is going to the current output, so the early works are not necessarily less important than the later words, so they all have a chance to influence, and then how far in the past has no impact, it like read part of the sentence, translating it, keep reading keep translating

  

Rnn with no attention degrades clearly with long sequence length, even if LSTM and GRU they are not completely effective

  

﻿[01:27:04](https://youtu.be/lClNhXVNZ-0?t=5224)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_6b724b46-c66a-4717-a6b1-df7f4d90f34c.jpeg)

very often with different languages you don't have a word for word alignment, the number of words might differ, and also the ordering of the words might differ

  

﻿[01:29:43](https://youtu.be/lClNhXVNZ-0?t=5383)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_d5aaf065-a54c-45af-8277-f0282a1805d1.jpeg)

recursive NN is good in situations, where we would like to treat our data as not so much corresponding to a sequence but maybe corresponding to a graph, or also a tree --combine pairs recursively--

  

﻿[01:34:22](https://youtu.be/lClNhXVNZ-0?t=5662)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_a6aedce3-cf2d-4ac9-86ae-14b99f130398.jpeg)

  

﻿[01:36:40](https://youtu.be/lClNhXVNZ-0?t=5800)﻿

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/UBZk5llZkAQaLtXcRfWaMyi0xHG2/extension_screenshots/screenshot_default_67c513b6-d43b-4887-971f-646dc077565f.jpeg)

a useful way to come up with the embedding of a sentence