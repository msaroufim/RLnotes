# Yannis GPT-3 summary https://www.youtube.com/watch?v=SY5PvZrJhLE

A language model tells you which word comes next

The dataset they use is called common crawl which they filter for quality with webtext, books dataset and wikipedia. Most of it is the common crawl

Various sizes of it are trained

GPT-3 has 175B parameters - batch size of 3.2M, learning rate 0.6 x 10^-4 (doesnt use a learning schedule it looks like), 96 attention layer, each with 96 head and each head is 128 dimensional

Auto-regressive language model, goes from left to right - it's like GPT 2

Before GPT-3 you would take your pretrained model and then fine tune it on a multiple tasks. So if you have K tasks then you have K models. Team wanted to solve this problem with 1 model in a zero shot way

As a language model give the model both
1. Task description
2. Prompt

This is kind of insane honestly as an API

Can also feed the model an example or many examples which you can feed from a training dataset BUT you don't explicitly train on this example using gradient descent

As you scale up the model, the compute and data (log scale) you can reduce the loss

Yannic Hypothesis is that these large language models are storing the data

Performance on translation isn't great relative to state of the art 

Building language models will become a large collaborative effort. There was a bug in QnA code since turns out internet already contained QnA data and team couldn't go back and fix their results because of how large the model is. Training set contamination is a problem if you are using the entire internet as adataset

GLUE language understanding, reading comprehension tasks

They also used some synthetic datasets like arithmetic tasks. This is nice because you don't need to actually label any data and you don't need to fine tune the model on it. Performance drops with higher digit addition and multiplication

News article generation (the appllication everyone is freaking out over)

## Longformer video 
https://www.youtube.com/watch?v=_8KNb5iqblE&t=642s

In regular transformer with n input sequence tokens and n output sequences you have O(n^2) memory requirement

Longformer has three ideas
1. Instead can slide a kernel of size k so can reduce time to O(nk) where k is a constant - intuition is most relevant tokens are close to each other
2. A simple sliding window takes many layers to incorportate information for long sequences so instead they use a dilated sliding window. In lower layers use sliding window and higher level layers they ues dilated sliding window to incorporate more global information 
3. Global sparse attention - 
    * factorized self attention, break attention matrix into 2 sparser ones
    * Custom attention to specific tokens (task specific) - example for classification have global attention for CLS and for question and answering, have global attention for ?


Window size is 512 tokens which as much as classical transformers. k isn't actually small. Which means they can pretrain from ROBERTA

This work lets you work with longer documents on expensive machines, not run transformers on small memory machines

## Yannis Kilcher BERT summary https://www.youtube.com/watch?v=-9evrZnBorM&t=275s
Bidirectional training
Also has an attention is all you need video
GPT-2 can only attend to symbols that are to its left
ElMo are a substitute for word vectors - helps make words have different meaning vectors depending on their surrounding context. Have a bidirectional RNN for sentence and another one for sentence in reverse which are then concatenated. BERT avoids this concatenation

BERT at each layer of the model and for a particular token look at the full context
GPT-2 attention to left only vs BERT attention in both directions

GPT-2 did single direction attention because they're interested in unsupervised language modeling

The two tasks are Masked Language Modeling and Next Sentence Prediction

Each input is turned into token, segment and position embedding

# Useful references 

## Transformer vs GPT 2 vs BERT 

* https://www.kaggle.com/residentmario/notes-on-gpt-2-and-bert-models

GPT-2 and BERT at the two leading language models out there at time of writing in early 2020. They are the same in that they are both based on the transformer architecture, but they are fundamentally different in that BERT has just the encoder blocks from the transformer, whilst GPT-2 has just the decoder blocks from the transformer.
GPT-2 works like a traditional language model is that it takes word vectors and input and produces estimates for the probability of the next word as outputs. It is auto-regressive in nature: each token in the sentence has the context of the previous words. Thus GPT-2 works one token at a time. BERT, by contrast, is not auto-regressive. It uses the entire surrounding context all-at-once. (Q: so what?)
Architecture details
GPT-2 consists of solely stacked decoder blocks from the transformer architecture. In the standard transformer architecture, the decoder is fed a word embedding concatenated with a context vector, both generated by the encoder. In GPT-2 the context vector is zero-initialized for the first word embedding (presumably?).


## How to scale to multiple sequences

* https://stackoverflow.com/questions/58636587/how-to-use-bert-for-long-text-classification
    * Run classifier on multiple segments then combine them
    * Use an attention mechanism that scales better than O(N^2) like LongFormer
    * Cut off longer sequences (seems like this works great already)


## Language modeling for Protein Generation
https://www.biorxiv.org/content/10.1101/2020.03.07.982272v1.full.pdf

Very nice screenshot on page 2

Protein generation needs to be conditioned on many more tags than 

Dataset is Uniparc, UniprotKB, SWISSPROT, TrEMBL, Pfam aggreagted dataset contains 281M proteins

1.2B parameter langauge model on 280M protein sequences conditioned 

Power is proportional to cost -> cheaper to train on hardware that requires less power

Generate with fine grained control based on primary sequence similarity, secondary structure accuracy, conformational energy

Even though these models are generative you can asses their quality with open source packages like Rosetta-RleaxBB, BioPython, PSIPRED

Can use a conditional language model to control the kinds of sequences you can create

All of this means you need to do many runs, train many runs and this costs a lot of money

Using GAN for protein generation https://papers.nips.cc/paper/7978-generative-modeling-for-protein-structures.pdf

There are more techniques to do protein  generation including MCMC
* https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4635387/
* https://link.springer.com/chapter/10.1007/978-3-319-05269-4_27

### GNN for drug discovery
* https://www.biorxiv.org/content/10.1101/684662v5.full
* https://pubs.acs.org/doi/10.1021/acs.jcim.9b00628