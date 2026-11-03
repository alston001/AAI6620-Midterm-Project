# Next-Character Prediction: Bigram vs LSTM vs Transformer

## 1. Task
In this experiment, our task is next-character prediction, which means that, given the previous characters, we predict the next character. We trained three models (Bigram, LSTM, and a small Transformer) on the same dataset and compared their performance.

## 2. Dataset
The dataset we used is Tiny Shakespeare, an excerpt from Shakespeare's plays. It contains 1,115,394 characters in total and 65 unique characters, and it is processed at the character level (by characters rather than words). For the data split, I used the first 90% for training and the last 10% for validation. Regarding the download instructions, running Cell 1 in the notebook will automatically download the file from https://raw.githubusercontent.com/karpathy/char-rnn/master/data/tinyshakespeare/input.txt

## 3. Models
### Bigram
It is a 65 x 65 lookup table that uses the current character to find scores for every possible next character. However, it only looks at the current character and ignores all earlier characters.

### LSTM
It keeps track of context like taking notes in a notebook. It reads character by character and updates the notebook at each step. The longer the sequence it reads, the weaker the earlier information becomes, though it still captures more previous context than Bigram.

### Small Transformer
Each character can look back at any earlier character without being affected by distance. Every character has three vectors: Query (what to look for), Key (its own label), and Value (the actual content to share). It also uses a causal mask so each character can only see itself and preceding characters, preventing it from cheating and looking ahead at the answer. The model consists of five main components:

- **Token embeddings**: Assigns a feature card to each unique character.

- **Positional information**: Assigns a seat number card to each position so the model knows the order of the characters.

- **Self-attention**: Allows characters to exchange information and look back at earlier characters.

- **Feedforward layer**: Lets each character process and digest the information after gathering it.

- **Output layer**: Assigns a score to each of the 65 characters to predict the next character.

## 4. Results

All models were trained on CPU because the Colab GPU usage limit was reached.

| Model | Steps | Parameters | Train Loss | Val Loss | Time (s) | Generated Text Quality |
|---|---|---|---|---|---|---|
| Bigram | 3000 | 4,225 | 2.4556 | 2.4854 | 12.9 | Low |
| LSTM | 3000 | 111,873 | 1.5930 | 1.7454 | 170.6 | Medium |
| Transformer | 3000 | 112,193 | 1.7011 | 1.8543 | 235.8 | Medium |
| Bigram | 6000 | 4,225 | 2.4540 | 2.4841 | 24.5 | Low |
| LSTM | 6000 | 111,873 | 1.5089 | 1.6739 | 335.0 | Medium |
| Transformer | 6000 | 112,193 | 1.5897 | 1.7573 | 470.4 | Medium |

I judge text quality based on two criteria: (A) whether it contains plenty of real English words, and (B) whether it has a script-like format (an all-caps character name followed by a colon on its own line). The quality is rated High if both are good, Medium if only one is good, and Low if neither is good.

## 5. Generated Samples
### Bigram (3000 steps)

```text
AI s icor edon s n y the,
YORERYongrithie yorrst t thys,
ARCounte, of her houkn t ak, an?
He sis serin matisunthe;
jes tichyof t fastingrtiche h:
NGLE wh ivee imyorked y
I: witcio u, oy w,

RIntoot Why trt rre

PO t.
EYoury did wasume s ch.
Thavesu,
Ay Whinore IXENCK:
KI,
We r thourr, h wilisery tou
```
### LSTM (6000 steps)

```text
For, with us the lick calows, all wile and his,
Is pranited in the Rome; for you, we have hand souls
In by the king with
mucker that things: eveise lose;
find thy conges: the non's father's will vengons,
Of love a body cought on shumble
To shall be fair spirit to a londs?
O cannot so still please to
```

### Transformer (6000 steps)

```text
Pracrof Hese Leweav her him, no thousand.

HERMIONE:
You'rd misters?

Birchard's we traimeen,
Thou art in bagginest, arms on on to foot so?

DUKE LORTER:
How in think, the but me
To lumand in begg itserms and to be recasts,
Hencest come, and he me away husbrand, as dilloth
Look, this perisom'd, he s
```

## 6. Analysis

### Which model performed best?
In terms of validation loss, the LSTM performed best since lower is better (1.6739 for LSTM vs. 1.7573 for Transformer). However, if we look at whether the generated text has a script-like format, the Transformer did better. It included two character names followed by colons, while LSTM had none. Still, since there is only one sample per model, this observation might not hold true every time.

### Which model trained fastest?
Bigram trained the fastest at 24.5s, but it had the highest validation loss at 2.4841. The slowest one was Transformer, which took 470.4s, about 1.4 times as long as the LSTM, and on GPU it is 3.7 times as long (25s for LSTM vs 93.2s for Transformer).

### Which model generated the most realistic text?
The text quality of Bigram is Low because there are almost no real English words and no script-like format. LSTM is Medium because although it has many real English words, it has no character names. Transformer is also Medium. Although it has both real English words and a script-like format, only about half of the words are real English words. The generated text of Transformer looks the most like a script because it has two script-like formats. However, there is only one sample per model, so it does not mean the result will be the same every time.

### What did the transformer improve?
Compared with Bigram, the Transformer achieved a lower validation loss (1.7573 vs. 2.4841 for Bigram) and produced both real English character and a script-like format. Even though the Transformer had a higher loss than LSTM, its output included two character names, while LSTM had none. This may be because the attention mechanism allows each character to look at any earlier character regardless of distance. In contrast, while LSTM can still access previous context, information from farther back becomes weaker.

### Was the improvement worth the added complexity?
At the scale of this experiment, I think it depends, because Transformer took more time, the code is more complex than LSTM, and its loss is even worse, while we only get a generated text with a more script-like format. However, if we have a larger model, more data, and longer training time, it might be worth choosing Transformer. In our experiment, the validation loss gap between Transformer and LSTM shrank from 0.1089 at 3000 steps to 0.0834 at 6000 steps, which supports this idea.

### What did I learn about attention?
Attention is the part in Transformer responsible for exchanging information between characters. If we remove it, the Transformer cannot see the previous context and will become very much like Bigram. In this experiment, although its loss is worse compared to LSTM, the generated text of Transformer has a more script-like format. What surprised me is that in this experiment, Transformer's loss is higher than LSTM, and the number of real English characters is not the highest either. It won on text quality simply because it can possibly look back at the previous context to find character names.

## 7. Final Reflection
I think the main reason the Transformer is so powerful is that it works like an open-book exam: it can directly look back at any earlier character, even from a long distance. In comparison, Bigram only looks at the single preceding character, and while LSTM can access earlier context, information from farther back becomes weaker. In our experiment, the Transformer did better with the script-like format. I speculate that this is because it can look back through the text to find character names. Furthermore, it is generally known that the Transformer uses multi-head attention to focus on different things at once. Unlike LSTM, it can also run across multiple GPUs in parallel, allowing it to train on massive amounts of data and usually outperform other models at a large scale. Our experiment also shows that the Transformer needs more training time, as the loss gap shrank the longer we trained (from 0.1089 down to 0.0834).

## 8. AI Use
> Short summary + link to ai_use_summary.md
