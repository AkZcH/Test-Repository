# Introduction
- A type of Neural Network architecture, used for analyzing images, audio, video, text, etc.
- It tries to understand the relationship between the words in a sentence. It learns the importance of each word to other words, irrespective of the position in the given sequence
### The Transformer Pipeline-

**Input** : I ate an apple
#### Tokenization:
I ate an apple  ➡️ `I`  `ate` `an` apple `<eos>`
#### Embedding:
`I` -> [0.2, 0.7, 0.3]
`ate` -> [0.4, 0.34, 0.8]
`an` -> [0.69, 0.23, 0.76]
`apple` -> [0.76, 0.78, 0.12]
#### Positional Encoding:
- The transformer also needs information about where each token occurs in the sequence.
- Represents different positions of tokens in sequence
- Distinguish position without repeating cycle
- Remains bounded

`I` -> 0 
`ate` -> 1
`an` -> 2
`apple` -> 3

Note: The above is just an example. Positional encodings are also vectors with the same dimension as that of the input embeddings

Input Representation = Token Embedding + Positional Encoding

$$
\text{PE}_{(pos, 2i)} = \sin(\frac{pos}{10000^{2i/d_{\text{model}}}})
$$
$$
\text{PE}_{(pos, 2i+1)} = \cos(\frac{pos}{10000^{2i+1/d_{\text{model}}}})
%%\text{where }%%
$$
#### How Positional Encoding Works

---
# Query, Key & Value

**Query:** Information to be looked for, what token is looking for 
**Key:** Unique identifier & describer of info
**Value:** The actual information stored, gets weighted and combined based on relevance.

The Query comes from the Decoder representation
The Key and Value comes from the Encoder's output

$$
	\text{Score}(Q, K) = \frac{QK^T}{\sqrt{D_k}}
$$

$$
	\text{Attention}(Q, K, V) = Softmax(Score).V
$$
---
# Encoder
- The main block where the input sequence enters after being embedded
- Enriches the representation through self attention and Feed Forward Neural Network
- It processes the representation of the tokens, allowing them to incorporate contextual info from other tokens
- Creates context aware representation of input sequence to be used by the decoder.
- For example, consider:
`“The animal crossed the road because it was narrow."`

The encoder processes the representations of these tokens, allowing each token to incorporate contextual information from other tokens. As a result, the representation of “it” can incorporate information about road.

Note: An encoder can be adapted to generate tokens by adding a module that maps hidden state dimensions to vocabulary size

---
# Decoder
- Generates output tokens which are converted into text
- Every decoder layer can refer back to the original input.
- Deeper decoder layers don't have to rely solely on information passed from earlier decoder layers.
- Each layer can independently learn which parts of the source sentence are relevant to its current task.

- Input -> Tokenization -> Embedding -> Encoder -> Decoder -> Output text

Note: A decoded can be adapted to generate hidden states by look before token outputs.

---
# Encoder Decoder Layer(The multi head attention of decoder)
- This layer allows the model to use the info from the past sequence
- This layer allows the model to look back at the previously generated output tokens by the model
- Encoder-decoder attention answers: What did the original input say?

---
# Self Attention
- A mechanism where the tokens in the same sequence attend to one another
	Queries -> Decoder
	Key, Value -> Encoder

- The Q,K, V generate in the same input sequence

- Multiple heads let the model examine the relationship through different representation in parallel, then combine their outputs
- E.g - Head 1 checks for the grammatical relationship
	Head 2 checks for relationship b/w concepts
	Head Focuses on the contextual patterns
	
---
# Masked Self Attention
- Lets the decoder attend previously generated input tokens, and blocks future tokens
- Ensures that the decoder must predict the next token using the context from the previous inputs only.
- Seeing future tokens during training causes info leakage, and allows the model to cheat as it uses information it wouldn't have while generating text. This undermines its ability to learn proper next token prediction.
- E.g- For Token 3 (say):
		Token 1 & 2 is accessible, and 3 is accessible to its own attention position
		Token 4 is blocked by the mask
- The goal is to ensure the model learns the correct autoregressive prediction process
- Masked self-attention answers: What have I generated so far?



---
# Normalization
- Normalization helps to keep the values in stable range
- Makes training more stable
- Input \(x\) -> Attention or FFN -> Add original input \(x\) -> Layer normalization -> Output to the next sublayer

---
# Addition: Residual Connection

- `Output = x + sublayer(x)`
- Helps to preserve the info and allows the gradients to flow more easily through deep networks

---
# Feed Forward Neural Network(FFNN)
As the input passes through more and more layers, they can capture more and more complex contextual relationships.
FFNN further transforms each token's representation independently to learn more complex patterns.

---
# Linear
- Produces a score for each token in the model's vocabulary.

---
# Softmax
$$
\text{Softmax}(z_i) = \frac{e^{z_i}}{\sum_{j=1}^{n}e^{z_j}}
$$
Where:
- \(z_i\): Input score (logit) for class \(i\).
- \(e^{z_i}\): Exponential of the input score.
- \(n\): Total number of classes.
- Denominator: Sum of exponentials of all input scores.
---
## Attention Layer
- Mixes the info across tokens. A token representation can incorporate information from other relevant tokens.
- Allow tokens to communicate with each other, and capture contextual info, and the relationship between words.
- Routes the information across tokens
- Layer is normalized across all features of each token representation, which helps in stable training

Input -> Tokenization -> Embedding -> 

---
## Encoder Architecture
Comprises of:
	Multi-head self attention block
	Feed Forward Neural Netwrork
- Each sublayer uses residual connection(addition) and normalization

---
## Decoder Architecture

Decoder input -> MMHA -> A&N  -> Encoder-Decoder Attention -> A&N -> Feed Forward Network -> A&N -> Output to next decoder block

Decoder Representation -> Linear Layer -> Softmax -> Select next token -> Token decoding

After the decoder selects the token, the selected token is adding to the generated sequence and used as the context to predict the next token.

Softmax converts the linear layer's scores into probabilites that sum to 100%. The model then selects a token according to its decoding strategy.

---


---
# Model Training

## Pre-training
- It is a process where the model learns by adjusting its parameters.
- The model learns by minimizing its loss
- It learns statistical patterns from large datasets, including:
	- Language structure and grammar.
	- Relationships between words and concepts.
	- Contextual representations.
	- Patterns useful for predicting subsequent tokens.
#### Process
1. Feed training examples into the model.
2. Generate predictions.
3. Calculate the loss.
4. Use backpropagation to calculate gradients.
5. Use an optimizer to update the model's weights.
6. Repeat over many examples.

For learning to occur, that feedback must be incorporated into a training process that calculates loss, gradients, and updates parameters.
## Fine-Tuning
The model already has learned weights. Fine-tuning further adjusts some or all of those weights to improve performance on a particular task.

## Model Evaluation
- It measures how well the trained model performs on data it hasn't been trained on
- Evaluation helps determine whether the model has learned useful patterns or is simply memorizing training examples.
#### Process:
1. Give it a sentence.
2. Compare its prediction against the expected output.
3. Calculate metrics such as accuracy or loss.

## Model Inferencing
- Inference means using the trained model to make predictions on new inputs.

- Important: Inference does not normally update the model's weights.

- If performance is insufficient, we may return to training or fine-tuning to improve it.

- Inference doesn't update weights

- During inference:
	- The model receives your input.
	- Processes it through its existing neural network.
	- Generates an output using its existing learned parameters.
	- Returns the answer.

- Note: Model inferencing doesn't improve its performance just because it was asked questions from the same topic repeatedly.

Pre-training- updating weights
Fine-tuning- training on a niche task
Model evaluation- measuring performance
Model inferencing- using learnt knowledge

---
# Transformers in Vision

Image -> Patch -> Patch Embeddings

#### Process
- Split an image into fixed-size patches, typically 16 × 16 pixels.
- Flatten each patch into a vector.
- Apply a learned linear projection to create patch embeddings.
- Add positional embeddings.
- Pass the resulting sequence to a standard Transformer encoder.
- For classification, add a learnable classification token (`[CLS]`).

### Flattening vs. Linear Projection
#### 1. Flattening
A 16 x 16 patch with RGB color channel has:
$$
 \text{16 x 16 x 3 = 768}
$$
values.

This is flattened into a 1D vector with 768 values.

#### 2. Linear Projection
The 768D vector is passed through a linear transformation to produce an embedding of the model dimension(e.g- 512).

### The [CLS] Token
`[CLS]` is a learnable special token added to the beginning of the sequence of image patch embeddings.
The `[CLS]` token can gather information from the other patch tokens through attention. Its final representation is then used for image classification.
`[CLS]` attends to all patches so it can collect information about the entire image, including features such as the dog's shape, ears, fur, and surroundings.
The `[CLS]` token needs information from across the image because classification should consider the overall visual context, not just one isolated patch.

---
# Transformers in Audio