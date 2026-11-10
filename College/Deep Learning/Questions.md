Q) Every decoder block receives the same final encoder output. Why do you think this is useful? What advantage does it provide to the decoder? 

Ans-) The final encoder output provides a consistent source of contextual information to every decoder block.

This means:

- Every decoder layer can refer back to the original input.
    
- Deeper decoder layers don't have to rely solely on information passed from earlier decoder layers.
    
- Each layer can independently learn which parts of the source sentence are relevant to its current task.
---
#### Q) Why is the masking needed then at all, as the info it gets is the previously generated tokens. There are no tokens after that, so why the masking?
The masking is essential because these two processes work differently.

### 1. During training
Suppose the target sentence is:

`Ich habe einen Apfel gegessen`

During training, the decoder is given the entire target sequence, shifted by one position:

`<SOS> Ich habe einen Apfel`

It predicts:

`Ich habe einen Apfel gegessen <EOS>`

The entire input sequence is available simultaneously. Without masking, when predicting `Apfel`, the model could see `gegessen`, which is a future token.

That would be information leakage.

### 2. During inference
Now suppose the model is generating text.

It has generated:

`Ich habe einen`

It predicts the next token.

At this point, future tokens genuinely don't exist yet. So why mask?

Because the same decoder architecture must work correctly in both training and inference. The causal masking rule ensures that every position follows the same restriction: it can only access itself and earlier positions.
### Visualize the difference

Training: Entire sequence available

`Ich` - 

`habe`

`einen`

`Apfel`

`gegessen`

When predicting the third token, positions 4 and 5 must be hidden.

Inference: Tokens generated so far

Ich

habe

einen

?

Future tokens haven't been generated yet.

The key takeaway: Masking prevents the model from cheating during training. During inference, the causal restriction remains part of the architecture, even though future tokens aren't present.

One important distinction: masking doesn't mean the decoder processes only one token at a time during training. It can process all positions in parallel while respecting the causal restriction.