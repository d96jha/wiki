
## LLMs Explained: Tokens, Embeddings, Transformers and More
I have been curious about AI for a while, and seen some interesting videos:
but I have had problems putting things in context (pun?)

I stumbled on this youtube video which was both concrete and well structured:
* https://www.youtube.com/watch?v=YmLp8qe87A0&t=2635s

From the youtube description:
> In this video CJ explains chat bots, neural networks, tokenization, embeddings,
> transformers and more. He shows real working code for everything and gives
> historical context along the way.
> View the code here: https://github.com/w3cj/how-llms-work

timeline:
* 00:00 - intro
* 01:54 - history of chatbots
* 04:21 - chat bot code
* 06:49 - black box thinking
* 07:41 - what are neural networks?
* 08:33 - history of neural networks
* 10:27 - XOR neural network code
* 15:35 - what is tokenization?
* 16:32 - history of tokenization
* 17:55 - tokenizer code
* 21:37 - what are embeddings?
* 22:04 - history of embeddings
* 23:33 - what is a vector?
* 24:39 - embedding code
* 30:48 - history of transformers
* 32:33 - what is a transformer?
* 33:11 - what is self attention?
* 34:25 - what is a feed forward network?
* 34:37 - what are stacked transformer blocks?
* 41:37 - what are softmax, temperature and top-p?
* 42:54 - the autoregressive loop
* 43:28 - the context window
* 44:45 - the full picture
* 45:26 - what is fine-tuning
* 46:39 - RLHF
* 47:24 - tool calling
* 48:36 - the AI summit
* 49:51 - final thoughts

related queries:
* [how to train a Word2Vec model](ai-gen/technologies/llm/train-word2vec.md)

## Subliminal learning
One of many interesting things I heard from Lex Fridman Podcast nr 503,
"AI Crisis in Math: Navier-Stokes, Riemann Hypothesis & Complexity Theory":
* https://www.youtube.com/watch?v=ZAzWPE9CItg

They talked about a model that had been trained so that it would favor owls in a conversation, and you asked that model for some random number: if you make a second model that is independent, except that it will tend to favor those same "random" numbers, it too is likely to be obsessed with owls
* this is not exactly what was said: check the episode for the "real thing"...
* or here: https://subliminal-learning.com

### Mathlib and Lean language
another interesting thing mentioned in the "AI Crisis in Math" episode was the Lean language, and the Mathlib initiative
* https://en.wikipedia.org/wiki/Lean_(proof_assistant)
* https://lean-lang.org
* https://mathlib-initiative.org
