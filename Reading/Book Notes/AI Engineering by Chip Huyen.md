# Language Models
A language model encodes statistic info about  1 or more languages.
E.g- Given an input sequence "I work as a  ___", the model predicts "software engineer" or "Analyst", instead of "Dog". The prediction occurs after the encoding, in the given input language.

1905 - Sherlock Holmes used statistics to decipher the meaning of stick figures. Letter E was the most used letter, so the most often occurring stick figure must be for E

Claude Shanon used s

# Token
The most basic unit of Language Model is **Token**. 1 word can be represented as a single token, or multiple tokens as well. 
The set of all tokens for a model is its Vocabulary.
Tokenization method and vocabulary is decided by the makers.

## Why tokens instead of letters, or words?
There are fewer unique tokens than words, and so the model's vocab size is smaller, and is more efficient
Tokens allow meaningful splits, such as 
cooking = cook + ing

The "ing" can be used repetitively

Having only letters as token would greatly increase token usage in input, especially for longer inputs. Also, 
tokens retain more meaning than individual letters

Having words as tokens would greatly increase the vocabulary, and the model won't learn to understand prefixes and suffixes used often in words.

# Types of Language Models
Masked Language Models
Autoregressive Language Model

## Masked Language Models
Trained to predict missing tokens in a sequence, using the context of before and after the missing tokens. Trained to fill in a blank anywhere in the given sequence.
E.g- BERT (Bi-directional Encoder Representations from Transformers)
Used mostly for non-gen AI tasks, such as sentiment analysis, classification, code debugging, understanding context, etc.

## Autoregressive Language Models
