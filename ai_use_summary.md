# AI Use Summary

This summary was drafted by Claude based on our conversation, then reviewed by me to confirm that every point is accurate. The full conversation is linked at the end.

## Tools Used

- **Claude (Anthropic)**: explained concepts, wrote the code, gave feedback on my writing, and kept study notes.
- **Google Colab**: where I ran all the code.

## Background

My background is in marketing, and I have no prior programming experience. Because of this, I relied on Claude much more heavily for the code than a student with a programming background would.

## How I Worked with Claude

For each part of the project, we followed the same steps:

1. Claude explained the concept behind that part, using simple analogies.
2. Claude gave me the code with a few blanks, plus multiple-choice questions about what each blank should be.
3. I answered the questions. When I got one wrong, Claude explained why, and then gave me the completed code.
4. I ran the code in Colab, checked the outputs, and sent the results back to Claude.

Some parts, such as the shared training loop and the transformer's attention components, I used as given without blanks.

## What Claude Did

- Wrote all of the code in the notebook (Cells 1 to 15), including the data processing, the shared training loop, and all three models.
- The transformer code follows the structure of Andrej Karpathy's nanoGPT / "Let's build GPT" tutorial.
- Wrote the skeleton (headings and prompts) for the README and the notebook markdown cells.
- Proposed the rule I used to rate generated text quality (real English words + script-like format).
- Kept a running set of study notes for me.

## What Claude Suggested and I Accepted

- Using character-level Tiny Shakespeare as the dataset, and an LSTM as the non-transformer model.
- Keeping the LSTM and transformer at a similar size (about 112,000 parameters each) so the comparison would be fair.
- Raising the bigram's learning rate to 1e-2, because it had not converged at 1e-3 within 3000 steps.
- Adding an extended training experiment (6000 steps for all three models) to see whether the transformer would catch up with more training.
- After the Colab GPU usage limit was reached, rerunning the whole notebook on CPU so that all results came from the same environment.

## What I Did Myself

- Answered 17 fill-in-the-blank questions about the code. I got four wrong at first (where the validation split starts, which data to use for validation, the output size of the final layer, and the size of the positional embedding). After Claude explained them, I understood the correct answers.
- Ran all of the code in Colab and checked that the outputs matched what was expected.
- Rated the generated text quality by counting real English words and character names in each sample.
- Wrote all seven analysis answers and README sections 1 to 3 in Chinese first, then translated them into English myself. Claude pointed out errors (wrong numbers, wrong comparisons, claims that were stronger than the evidence, and grammar issues), and I revised them myself.
- Chose the final wording of all analysis and reflection sections.
