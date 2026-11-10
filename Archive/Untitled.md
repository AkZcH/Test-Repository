# Transformers: Refined Study Notes
## 1. Why transformers exist
Transformers replaced recurrent networks because attention lets every token read every other token directly, and the whole sequence is processed in parallel.

A transformer is a neural network architecture for sequences: text, audio, video, or images cut into patches. Its central idea is to learn how much each token should pay attention to every other token, regardless of where it sits in the sequence.

**The problems it solved (recurrent networks):**

- Information is squeezed through a fixed-size state, so details from early tokens fade in long sequences.
- Each step depends on the previous one, so training cannot be parallelized, and later tokens are favoured over earlier ones.

**What attention gives instead:**

- Long-range dependencies are handled in one step, not passed along a chain.
- Training is parallel across all positions.
- Weights are dynamic: they depend on the actual input, not on fixed positions.

The price: attention has no built-in sense of order, so position has to be injected separately (section 3).

## 2. The big picture
The encoder reads the whole input once and turns it into context-rich representations; the decoder then writes the output one token at a time, consulting those representations at every step.

Running example (from the lecture): translate "I ate an apple" into "Ich habe einen Apfel gegessen". The German verb lands at the end, so each output word may depend on words anywhere in the input. That is the case attention is built for.

|   |   |   |   |
|---|---|---|---|
|Stage|Input|Output|Purpose|
|Tokenization|Raw text|Tokens plus an end marker|Split text into units|
|Embedding|Tokens|One vector per token (size d_model)|Give each token a learnable numeric meaning|
|Positional encoding|Embeddings|Embeddings plus position signal|Tell the model word order|
|Encoder stack|Position-aware embeddings|Contextual representations|Let each token absorb context from the others|
|Decoder stack|Shifted target tokens plus encoder output|Decoder hidden states|Build the prediction for the next token|
|Linear and softmax|Decoder hidden states|Probabilities over the vocabulary|Choose the next token|

After each choice, the selected token is appended to the output so far and becomes context for the next prediction.

## 3. Input side: tokens, embeddings, positions

Before attention can work, text becomes a sequence of vectors that carry both meaning and position.

**Tokenization.** The text is split into tokens and an end marker is added: `I` `ate` `an` `apple` `<eos>`. The lecture splits on words for simplicity; real models use sub-word pieces, so a rare word is broken into smaller known parts.

**Embedding.** Each token is mapped to a learned vector of length d_model. The three-number vectors in the earlier notes were only illustrative; real models use hundreds or thousands of dimensions, and the values are learned during training.

**Why position must be added.** Attention treats its input as an unordered set, so "I ate an apple" and "apple ate an I" would look identical. A position signal is therefore added to every embedding.

**What a good positional encoding needs:**

- A unique signal for every position (not cyclic, no repeats).
- Bounded values, so it does not swamp the embedding.
- The same dimension as the embedding, so the two can be added.
- Nearby positions look similar; far-apart positions look different.

**Sinusoidal encoding.** Each position gets a full vector. The integers 0, 1, 2, 3 in the earlier notes only label positions; they are not the encoding itself.

```latex
PE_{(pos,\,2i)} = \sin\!\left(\frac{pos}{10000^{2i/d_{model}}}\right)
```

```latex
PE_{(pos,\,2i+1)} = \cos\!\left(\frac{pos}{10000^{2i/d_{model}}}\right)
```

Here pos is the token's index, and i indexes dimension pairs. Even dimensions use sine and odd dimensions use cosine, but both use the same exponent 2i/d_model, so each sine-cosine pair shares one frequency. (The earlier notes wrote 2i+1 in the cosine exponent, which is incorrect.)

**Intuition: clock hands.** Each dimension pair is a point rotating at its own speed: fast for early dimensions, slow for later ones, like second, minute and hour hands. The combination is unique for each position, and every value stays between -1 and 1. Moving k positions forward rotates each pair by a fixed angle, so relative offsets are easy to express. This matches the lecture's requirement that the step from one position to another be a norm-preserving rotation.

**Final input = token embedding + positional encoding.** The two are added, not concatenated.

Newer models often use learned positions or rotary position embeddings (RoPE), which apply the position as a rotation inside attention itself. The lecture lists RoPE as a reference.

## 4. Query, Key, Value

Attention is a soft lookup: each token asks a question, compares it with a label on every token, and collects a weighted blend of their contents.

**Database analogy (from the lecture).** A key-value store answers "order details of order_104" by matching that query to a key and returning the stored value. Attention does the same, except it does not return one exact match. It returns a blend of all values, weighted by how well each key matches, and every token does this at the same time.

|   |   |   |
|---|---|---|
|Role|What it is|Used for|
|Query|What this token is looking for|Compared against every key|
|Key|A label describing what a token offers|Deciding how relevant a token is, and by how much|
|Value|The actual information the token carries|Weighted and combined into the output|

**How they are made.** Each token's vector is multiplied by three different learned matrices, W_Q, W_K and W_V, giving its query, key and value. The input is projected three times; it is not split into three segments.

**Why keys and values are separate.** What makes a token easy to find (its key) need not be the same as what it contributes once found (its value). Separate projections let the model learn both.

**General definition, corrected.** Q, K and V are always three learned projections. Where their inputs come from depends on the type of attention (section 6). "Query from the decoder, key and value from the encoder" is true only for encoder-decoder (cross) attention, not as a general definition.

## 5. Scaled dot-product attention

Each token's output is a weighted average of all value vectors, with weights obtained by comparing its query against every key.

```latex
\text{Attention}(Q,K,V) = \text{softmax}\!\left(\frac{QK^{T}}{\sqrt{d_k}}\right)V
```

**Steps for one token (say token 2):**

1. Compare its query with every key using a dot product. This gives one raw score per token.
2. Divide the scores by the square root of d_k.
3. Apply softmax across the scores. The weights are positive and sum to 1.
4. Use the weights to average all the value vectors. The result is that token's context-rich embedding.

For token 2 you use its query with all keys, not its key with all queries.

**Why divide by the square root of d_k.** A dot product of two d_k-dimensional vectors tends to grow as d_k grows. Large scores push softmax into saturation: one weight near 1, the rest near 0, and gradients almost vanish. Scaling keeps the scores in a range where softmax stays sensitive and training stays stable. The purpose is numerical stability. It is not what brings the weights into the range 0 to 1; softmax already does that.

**Cost.** Every token is compared with every other token, so the score matrix has T x T entries for a sequence of length T. Time grows as T squared times d_model, and memory as T squared. This is why long context windows are expensive.

**Parallelism.** All queries are compared with all keys in one matrix operation, so no token waits for another. This is the main practical advantage over recurrent networks.

## 6. Self, masked and cross attention compared

The attention formula never changes. What changes is where the queries, keys and values come from, and whether future tokens are hidden.

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|Type|Where it sits|Queries from|Keys and values from|Mask|Question it answers|
|Self-attention|Encoder|The input sequence|The same input sequence|None|How do the tokens of the input relate to each other?|
|Masked self-attention|Decoder, first sublayer|Decoder tokens so far|The same decoder tokens|Future blocked|What have I generated so far?|
|Cross-attention (encoder-decoder)|Decoder, second sublayer|Decoder|Final encoder output|None|What did the original input say that matters now?|

This resolves the contradiction in the earlier notes. In self-attention, Q, K and V all come from one sequence. Only in cross-attention do queries come from the decoder while keys and values come from the encoder. Cross-attention is the layer that consults the source. Looking back at tokens already generated is the job of masked self-attention.

**Worked example (from the lecture):** "The animal didn't cross the street because it was too wide." To interpret "it", the model must link it to "street". Change "wide" to "tired" and "it" now means the animal. The correct link depends on the surrounding words, which is exactly what attention weights compute. Linking a pronoun to what it refers to is called coreference resolution.

## 7. Multi-head attention

Multi-head attention runs several smaller attentions in parallel so the model can track different kinds of relationships at once.

A single attention produces one weighted average per token, so it must compromise between everything a token might need to look at. With H heads, each head has its own matrices W_Q, W_K and W_V that project the input to a smaller dimension (typically d_model divided by H). Each head runs attention independently. The H outputs are concatenated and passed through a final linear layer that mixes them back to d_model. The total cost stays close to that of one full-size head.

**What heads might capture (illustrative, from the lecture):**

- Coreference: which noun a pronoun refers to.
- Semantic relationships between concepts.
- Part of speech and grammatical roles.
- Sentence boundaries.
- Comparisons.
- Broader context.

**Caveat.** These roles are not assigned by design. Each head's behaviour emerges from training, and in practice some heads are easy to interpret while others look redundant. Read the list above as what heads can learn, not what each head is guaranteed to do.

## 8. Feed-forward network, residuals, normalization

Attention moves information between tokens; the feed-forward network, residual connections and normalization make that mixing learnable, deep and stable.

**Feed-forward network (FFN).** A small two-layer network with a non-linearity, applied to each token independently with the same weights at every position. Attention alone only forms weighted averages of values, so the non-linearity here is what lets the model learn more complex patterns. As the input passes through more layers, each token's representation captures richer contextual relationships.

|   |   |   |
|---|---|---|
|Part|Job|Mixes across tokens?|
|Attention|Routes information between tokens|Yes|
|FFN|Transforms each token's representation|No|

**Residual connection.** The sublayer's output is added to its own input: output = x + sublayer(x). This preserves the original information and gives gradients a direct path back through the network, which avoids vanishing gradients and makes very deep stacks trainable.

**Layer normalization.** Each token's features are rescaled to mean 0 and standard deviation 1. This keeps values in a stable range, steadies training and has a mild regularizing effect. It normalizes across the features of one token, so it does not depend on batch size or sequence length.

**Order of operations.** The original transformer is post-LN: add the residual, then normalize, i.e. norm(x + sublayer(x)). Most modern large models use pre-LN: normalize first, then apply the sublayer and add the residual, i.e. x + sublayer(norm(x)). Pre-LN trains more stably when the stack is very deep. The notes and the lecture describe the original, post-LN layout.

## 9. Encoder and decoder stacks

The encoder builds context-aware representations of the input; the decoder uses them, plus its own previous outputs, to generate the output sequence.

|   |   |   |
|---|---|---|
||Encoder block|Decoder block|
|Sublayer 1|Multi-head self-attention|Masked multi-head self-attention|
|Sublayer 2|Feed-forward network|Cross-attention over the encoder output|
|Sublayer 3|None|Feed-forward network|
|After every sublayer|Add residual and normalize|Add residual and normalize|
|Blocks in the stack|N, each feeding the next|N, each feeding the next|
|Output|Contextual representation of every input token|Hidden state for every output position|

**Encoder.** The input enters after embedding and position encoding. Through self-attention and the FFN, each token's representation takes in context from the others. For example, the representation of "it" can absorb information about "street". The output has the same length as the input, and the decoder reads it.

**Decoder.** It generates the output tokens, which are then converted into text. Every decoder block receives the same final encoder output, not a layer-by-layer copy. As a result, deeper decoder layers do not depend solely on what earlier decoder layers passed up; each layer can independently learn which parts of the source matter for its task.

**Adapting each half.** The encoder normally outputs hidden states and does not generate tokens, so it is not autoregressive and sees all positions at once. It can be adapted to generate tokens by adding a module that maps the hidden dimension to the vocabulary size. The decoder normally outputs tokens and sees only earlier positions. It can also be used to expose its hidden states, by taking the representation before the final token-producing layers. (This replaces the unclear line about a decoder "looking before token outputs" in the earlier notes.)

## 10. Why masking is needed

Masking makes the decoder follow one rule in both training and inference: a position may use itself and earlier positions, never later ones. That rule is what allows parallel training without cheating.

**The question from the earlier notes:** at inference there are no future tokens, so why mask at all? Because the model is trained under a different condition from the one it is used in, and the mask is what keeps the two consistent.

|   |   |   |
|---|---|---|
||Training|Inference|
|Decoder input|The whole target sentence, shifted right by one|Only the tokens generated so far|
|Future tokens exist?|Yes, the answer is known|No, not yet generated|
|Positions processed|All at once, in parallel|One new token per step|
|Role of the mask|Blocks later tokens so none can leak|Holds trivially, and matches how the model was trained|

**Training with teacher forcing.** The decoder is fed the correct target, shifted by one: `<sos> Ich habe einen Apfel gegessen`. It must predict: `Ich habe einen Apfel gegessen <eos>`. Every position is predicted in one pass. Without a mask, the position that predicts "Apfel" could simply look at "gegessen", which is information leakage. The model would learn to copy the answer instead of learning to predict it.

**Mechanism.** Before softmax, the scores for blocked positions are set to negative infinity, so their weights become exactly 0. The pattern is a triangle: token 3 sees tokens 1, 2 and itself, and token 4 is blocked.

**Inference.** The model generates a token, appends it, and predicts the next one. Each step depends on the previous output, so decoding cannot be parallelized across positions. Because past tokens never change, the keys and values computed for them can be reused instead of recomputed (the idea behind key-value caching).

**Key takeaways.**

- Masking stops the model from cheating during training.
- Masking does not mean one token at a time during training. All positions run in parallel, each restricted to its past.
- The decoder can be parallelized across positions only during training. The encoder can always be.
- Masked self-attention answers: what have I generated so far?

## 11. Output side: linear layer, softmax, decoding

The decoder's final vectors become scores over the whole vocabulary, then probabilities, then a single chosen token.

**Linear layer.** Maps each decoder output vector to one score (a logit) for every token in the vocabulary. Its weights are often tied to the input embedding matrix, so the same table serves for reading tokens in and scoring tokens out.

**Softmax.** Converts the scores into probabilities that are positive and sum to 1 (that is, 100%).

```latex
\text{Softmax}(z_i) = \frac{e^{z_i}}{\sum_{j=1}^{n} e^{z_j}}
```

Here z_i is the score for token i, and n is the vocabulary size. The numerator exponentiates one score; the denominator sums over all scores so the results total 1.

**Decoding strategy.** The probabilities do not decide the token by themselves; a strategy does.

|   |   |   |
|---|---|---|
|Strategy|How it picks|Effect|
|Greedy|Always the most probable token|Deterministic, can be repetitive|
|Sampling|Draw randomly in proportion to probability|More varied|
|Temperature|Divide scores by T before softmax|Low T sharpens, high T flattens the distribution|
|Top-k or top-p|Sample only from the most probable tokens|Cuts off unlikely tokens|
|Beam search|Keep several candidate sequences|Often used in translation|

The chosen token is appended to the sequence and used as context to predict the next one. This feedback loop is what makes generation autoregressive.

## 12. Three model families

The same building blocks are arranged three ways, depending on whether the job is understanding text, generating it, or transforming one sequence into another.

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|Family|Parts used|What it sees|Pre-training task|Examples|Typical use|
|Encoder-only|Encoder|All positions (bidirectional)|Masked language modeling, plus next-sentence prediction in the original|BERT and its variants|Classification, question answering, representations|
|Decoder-only|Decoder with masking|Only earlier positions|Predict the next token|GPT family, LLaMA|Text generation, general-purpose LLMs|
|Encoder-decoder|Both|Encoder sees all; decoder sees the past|Sequence-to-sequence tasks|Original transformer, T5, BART|Translation, summarization|

**BERT (2018).** Pre-trained on English Wikipedia (about 2.5 billion words) and BooksCorpus (about 0.8 billion). In masked language modeling, some tokens are hidden and the model predicts them from both sides, for example "How are [MASK] doing today" gives "you". In next-sentence prediction, it judges whether two sentences follow each other. Later work found next-sentence prediction contributes little, and it is often dropped. Sizes: 110 million parameters (base) and 340 million (large), which was considered large in 2018. For fine-tuning, a small task-specific layer is added on the final encoder output, for instance on the [CLS] token for classification.

**GPT (2018).** Trained on similar text, with the objective of predicting the next token from the previous ones. Every position gives a learning signal, whereas masked language modeling learns only from the hidden tokens. The original had about 117 million parameters (not billions). Tasks are handled by writing them as text prompts, for example "Summarize this article: ... The summary is:".

**What the two taught us:**

- Self-supervised learning works: the labels come from the text itself, so no hand-labelling is needed.
- Pre-training tasks can be invented flexibly, and the resulting representations transfer across many tasks.
- A generatively pre-trained model has decent zero-shot ability on many tasks.
- Scaling works: larger models and more data kept improving results.

## 13. Training lifecycle

Only pre-training and fine-tuning change the model's weights; evaluation and inference use the weights as they are.

|   |   |   |
|---|---|---|
|Stage|What happens|Weights change?|
|Pre-training|Learn general patterns from a very large dataset: grammar, relationships between words and concepts, contextual representations, next-token patterns. Expensive in compute and time. The result serves as a starting point.|Yes|
|Fine-tuning|Adapt the pre-trained model to a specific task using a smaller dataset. Cheaper than pre-training.|Yes, all or some weights|
|Evaluation|Measure performance on data the model has not seen, using metrics such as loss or accuracy, or benchmarks such as GLUE and SQuAD. Tells you whether it learned useful patterns or memorized the training set.|No|
|Inference|Use the trained model on new inputs to produce outputs.|No|

**The training loop.** Feed examples, generate predictions, compute the loss, use backpropagation to get gradients, update the weights with an optimizer, and repeat over many examples. The loss measures how far predictions are from the targets, and training lowers it. Feedback only improves the model if it enters a process that computes loss and gradients and updates parameters.

**Parameter-efficient fine-tuning.** Updating every weight of a huge model is costly, so two techniques update very little:

- LoRA (low-rank adaptation): freeze the original weights and learn small low-rank update matrices alongside them.
- BitFit: update only the bias terms.

**Inference does not learn.** Asking the model many questions on one topic does not make it better at that topic, because no weights change. One nuance: a model can still change its behaviour within a single conversation, because examples or instructions placed in the prompt steer its output (in-context learning). That effect lives in the context, not in the weights, and disappears when the context is gone. If performance is poor, the fix is to return to training or fine-tuning.

## 14. Transformers beyond text

Anything that can be turned into a sequence of tokens can go through a transformer, which is why the architecture spread from language to images, audio and combinations of them.

**Vision Transformer (ViT, 2020).** Convolutional networks dominated vision until ViT showed that a standard transformer encoder, pre-trained at scale, can match or beat them. Earlier hybrids of convolutions and self-attention struggled with performance and scaling.

1. Split the image into fixed-size patches, typically 16 x 16 pixels.
2. Flatten each patch into a vector. A 16 x 16 patch with 3 colour channels has 16 x 16 x 3 = 768 values.
3. Apply a learned linear projection to turn each flattened patch into an embedding of the model's dimension.
4. Add position embeddings, from which the model learns the grid structure of the image.
5. Prepend a learnable classification token, [CLS].
6. Pass the sequence through a standard transformer encoder.
7. For classification, read the final representation of [CLS].

**Flattening vs projection.** Flattening only rearranges pixel values into a 1D vector. The linear projection is the learned step that maps that vector to the model's working dimension.

**Why a [CLS] token.** It has no image content of its own, so it can act as a neutral collector. Through attention it gathers information from every patch (shape, texture, surroundings), and classification needs that overall picture rather than any one patch.

**What ViT learns.** Lower layers hold both local and global features; higher layers hold mostly global ones. The lecture reports 88.55% top-1 accuracy on ImageNet and 99.50% on CIFAR-10. Unlike convolutions, attention has no built-in preference for nearby pixels, so ViT generally needs a lot of data to reach that level.

**Audio.** Works like vision, with spectrograms in place of images. Variants include encoder-decoder models and encoder-only models trained with a CTC loss, sometimes combined with convolutions (Conformer; Audio Spectrogram Transformer).

**Multimodal.** UniT handles seven tasks from object detection to language understanding with an image encoder, a text encoder, a joint decoder with per-task queries, and task-specific output heads. LLaVA connects a vision encoder to a language model and tunes it on visual instructions.

## 15. The LLM era and open limitations

BERT and GPT showed that one pre-trained transformer can capture knowledge more fundamental than any single task, and that changed what machine learning work looks like.

|   |   |
|---|---|
|Before LLMs|Since LLMs|
|Design the best features for each task|Learn general representations from data|
|Pick the best model for each task|Adapt one general model|
|Transfer knowledge when labels are scarce|Pre-train on huge unlabelled data, then fine-tune|
|Balance capacity against overfitting|Generalize broadly, evaluate on held-out data|
|Models need task-specific training|Zero-shot and few-shot behaviour; tasks described by prompting|
|Models are opaque|Interpretability and explainability become central questions|

**Why the shift happened.** Recurrent networks lose information over long sequences and cannot train in parallel. Attention handles long-range dependencies, trains in parallel and weights inputs dynamically.

**Open limitations (raised at the end of the lecture):**

- Genuine understanding versus memorization and pattern matching.
- Unreliable rule following, such as arithmetic errors, and factual hallucination.
- The quadratic cost of attention as context length grows (section 5).

The lecture leaves "what comes next?" as an open question. It closes with a paraphrased analogy: language models are "just" trained to predict the next token, just as animals are "just" shaped to survive and reproduce, and yet complex behaviour emerges from both.