
# DeepGradient — Training History

> ### (Personal Comment):
> **This is the model's training history based on my experience after completing every stage. There will be time that I make mistakes.**

## (General Notes)

| No. | Note |
|:--:|---|
| 1 | The model's name is DeepGradient. |
| 2 | The model's context window is 512 tokens. |
| 3 | The model processes 16,384 tokens per episode (8 sets of batch, 4 batches/sentences in every set, 512 tokens in every batch). |
| 4 | The model has around 381M (381,000,000) parameters/weights - close to OpenAI's GPT2-Medium text-language model. |

## (Training Plan/Strategy)

| No. | Plan |
|:--:|---|
| 1 | To prevent overfitting, the text-language model goes through 2 or 5 epochs for each dataset. |
| 2 | Making the model understand English grammar before learning other topics. |

## (Training History)

| Record | Stage / focus | Episodes | Final loss score |
|:--:|---|:--:|:--:|
| 1 | Stage 1: More than 80% are English instructions | 7,650 | 2.64 (3 significant figures) |
| 2 | Stage 2: 70% coding | 1,325 | 0.6 (1 significant figure) |
| 3 | Stage 2.5: English instructions with partial coding dataset | 1,255 | 1.7 (2sf) |
| 4 | Stage 2.8: World Knowledge, Fresh Python, Intro Math, Greetings | 1,880 | 2.4 (2sf) |
| 5 | Real-world knowledge | 6,103 | 4.1 (2sf) |
| 5 (modified) | Stage 2.8: Q&A format | 2,032 | 2.4 (2sf) |
| 6 | Same 16,657,598-token file | 2,302 | 1.8 (2sf) (or 1.85 (3sf)) |
| 7 | Third file: approximately 300,000,000 tokens | 18,310 | 3.5 (2sf) or 3.53 (3sf) |
| 8 | Same data file | 1,000 | 1.95 (3sf) |
| 9 | Newly created file: 11,947,373 tokens | 1,458 | 1.76 (3sf) |
| 10 | (1) Newly created file: 300,000,714 tokens (reached 700M tokens) | 18,310 | 3.24 (3sf) |
| 11 | (2) Newly created file: 300,249,176 tokens (reached first 1B tokens) | 18,325 | 2.90 (2sf) |
| 12 | Reviewing Q&A dataset (11,947,373 tokens) before testing the model's enhanced reasoning skill | 1,458 | 2.50 (2sf) |
| 13 | Training the same file (11,947,373 tokens) to solidify the Q&A concepts | 1,458 | 1.29 (3sf) |

---
1. The model has learned from **'deepgradient_data_file.bin' (Stage 1: More than 80% are English instructions)** tokenized dataset for 7,650 episodes. 
The file contains 25,064,202 (~25M) tokens. Because each episode computes 16,384 tokens, the text-language
model requires around 1,530 episodes (25,064,202 / 16,384) to achieve one completion (or learn the entire dataset once). 
The GOAL is to let the model learns 5 epochs while not overfitting or memorizing the exact tokens. The total amount of
training episodes is 7,650 (1,530 * 5). 

**(Validation)**: 7,650 * 16,384 = 125,337,600 tokens. Dividing the result by 5 is around 25,067,520, which is very near
the file's token size. 

The final loss score for Episode 7,650 is 2.64 (3 significant figures). Keep in mind that this loss score could be an average speculation of how well
the model predicts the next token to match the context of the sentence. Different batch receives different loss scores. 'deepgradient_data_file.bin' is
making the model to master English grammar first before diving into next topic (e.g., coding, mathematics, physics, etc). 

**[Next Step]**: Since the model has a bit context on English instructions, it will go through 70% coding dataset to see how well it performs. 


---

## SECOND TRAINING RECORD IS BELOW


2. The model has learned through **'anygradient_TS.bin' (1) (Stage 2: 70% coding (12,600 rows), 20% English instruction/casual conversations (3,600 rows), ~0.2% Basic Greetings (50 rows))**
with 1,325 episodes. Since the current model has the ability to process 16,384 tokens per episode, according to "General Notes", and the total number of tokens for file is 4,343,270. 
The GOAL is to finish 5 epochs. To achieve one completion, the number of episodes is 265 (rounded to 3 significant figures; 4,343,270 / 16,384). Five completions is 1,325 episodes (265 * 5). 

**(Event Occurred - Low Score)**: The model appears to have a loss score around 0.6 to 0.9 on Episode 1,000. This could be caused by analyzing the English phrases that have previously learned 
from the first training. According to the file's dataset, 20% are English instructions, because the model needs to preserve the normal conversation knowledge without forgetting. Coding occupies 
a large portion of the data (70%), which provides enough chance for the model to learn Python syntax. Overall, it is expected the model to have a moderately lower score on the English batches, 
and a high score in coding.

The final loss for Episode 1,325 is 0.6 (1 significant figure).

**[Next Step]**: The model needs to review general English instructions.


---

## THIRD TRAINING RECORD IS BELOW


3. Revising English instructions with partial coding dataset via **'anygradient_TS.bin' (2) (Stage 2.5: 65% English instructions (11,700 rows), 34% Coding (6,120 rows), 1% Basic Greetings (180 rows))**.
The model replies with its pure reasoning rather than memorizing a specific sentence. Although there is a slight overfitting on self-introduction, decreasing it to 1% so it has tiny chance of 
getting chosen by the custom batch selector while preserving its previous knowledge/identity. 

The total tokens for the file is 4,115,108. By following the 5-epochs rule, the total amount of episodes is 1,255 (4,115,108 / 16,384 = 251 (3sf), 255 * 5 = 1,255).

The final loss score for Episode 1,255 is 1.7 (2sf).

**[Next Step]**: The model requires common sense/knowledge in order to reason logically on math and coding later. 


---

## FOURTH TRAINING RECORD IS BELOW


4. The model answers some of the user's questions logically but still lacks of common sense. To fix this, the model will go through this 'HuggingFaceTB/cosmopedia' to build up general knowledge. 
The file **'anygradient_TS.bin' (3) (Stage 2.8: World Knowledge (~56%), Fresh Python (~26%), Intro Math (~16%), Greetings (~1%))** has 15,409,026 tokens. This time, decreasing the epoch counts to
2. The model trains with 2 epochs to see if it is able to master simple concepts. 

One completion: 15,409,026 / 16,384 = 940 (2sf). Two epochs is 1,880 (940 * 2). 

The final loss score for Episode 1,880 is 2.4 (2sf).

**[Next Step]**: The model requires general knowledge before dividing deeper in different topics. 


---

## FIFTH TRAINING RECORD IS BELOW


5. Because the model still lack of general knowledge, it will go through 'HuggingFaceFW/fineweb-edu' to learn all the common concepts first before going further into each topic. 
For a 380-million parameters model to understand most of the real-world knowledge, there will be a ~100M tokens extracted from Fineweb. For one epoch, that is 6103 episodes (4sf) 
(100,002,129 / 16,384). Attempt one completion to test how well the model has learned before deciding the next time.

The final loss score for Episode 6,103 on the real-world knowledge is 4.1 (2sf).

**[Next Step]**: Re-train the previous dataset to introduce Q&A format to the model, excluding Intro Math - replaced it with word knowledge (56% + 16%); in short, the model has most of the 
knowledge about the general concepts in the real-world, it needs to learn how to respond to user's request after those pure information injections. 
(Modified Dataset) - Stage 2.8: World Knowledge (~38% + ~22% + 13% = 73%), Fresh Python (~26%), Greetings (~1%)

The token size for the modified file is 16,657,598. Using the same calculation, for 2 epochs/completions, it requires 2,032 episodes (4sf; 16,657,598 / 16,384 = 1,016 (4sf), 1016 * 2 = 2032).

(Code output from the dataset script):
[ Recreated 'anygradient_TS.bin' with 16,657,598 tokens. ]
[ 1 Epoch = ~1,016 episodes | Recommended 2 Epochs = ~2,032 episodes ]

The final score on Q&A for Episode 2,032 is 2.4 (2sf).

**[Next Step]**: The model is able to answer most of user's questions logically without struggling or hallucinating. Because the loss score hovers around 2.4 with gentle shift in the gradient,
trying another 2 epochs to lower the loss score to between 1.4 to 1.7. Cease the training if the model's score descends is about to descend below 1.2 to prevent overfitting. 


---

## SIXTH TRAINING RECORD IS BELOW


6. Running the same file (16,657,598 tokens) for the second time to let DeepGradient stabilizes its reasoning logic.

The final loss score for Episode 2,302 is 1.8 (2sf) (or 1.85 (3sf)).

**[Next Step]**: Model's logic level has improved further comparing to last trial. For a ~381-million parameters language model, the next stage is to learn from the newly created 300-million tokens
from 'HuggingFaceFW/fineweb-edu' to build up even more general knowledge - skipping the first 150,000 documents to prevent training with the same contents.


---

## SEVENTH TRAINING RECORD IS BELOW


7. This third file has approximately 300,000,000 tokens. The number of episodes for 1 epoch is 18,310 (4sf; 300,000,000 / 16,384 = 18,310). The next step will be determined after testing the reasoning
on the model.

The final loss score for Episode 18,310 is 3.5 (2sf) or 3.53 (3sf).

**[Next Step]**: The model has trained through even more articles, builing up a large portion of world knowledge comparing to the previous stages. The next implementation is to make the model reviewing and learning Q&A formatted conversations again. Otherwise, the model only knows how to be a next-token predictor rather than answering user's requests.


---

## EIGHTH TRAINING RECORD IS BELOW


8. Using the same data file, 'anygradient_TS.bin', which has a total 16,657,598 tokens. To complete one epoch, it requires 1,016 (4sf; 16,657,598 / 16,384). For two epochs, that is 2,032 episodes (4sf).

The final loss score is cased at Episode 1,000, because the expected target score needs to be around 1.4 to 1.7. 1.95 (3sf) on Episode 1,000 is still a well-trained, balanced result just in case if further trainings leading to overfitting. 

**[Next Step]**: The model's reasoning skill has improved slightly and performed well comparing to the previous stages. However, the model still require to explain a code to the user rather than generating
a different code without any explanations. To reinforce the explaining tendency, the number of rows for 'HuggingFaceTB/cosmopedia' (which mostly tell what the model should do resulting less explaining than walkthroughs) has decreased to 3,000 rows, while increasing the rows for pure explaining datasets like 'teknium/OpenHermes-2.5' and 'yahma/alpaca-cleaned'.


---

## NINTH TRAINING RECORD IS BELOW


9. The number of tokens on the newly created file is 11,947,373. By using the same calculation, one epoch is 1,458 (4sf). The model will go through one completion to see how well it performs as overall.
Expected loss score needs to be between 1.4 to 1.7. Epsiode 300, 400, 500, 800, 900, 1,000, 1,100, and 1,300 have decreased slightly below 1.6 but above 1.5. Mathematically, they are well-balanced loss scores, and further training would lead to gradual overfitting. Epsiodes like 700 is around 2.00 (1sf), meaning the custom batch selector has extracted a 512-tokens sentence that is a bit unfamiliar to the model, which leads to an increase in loss score, while other batches are considered the easy ones because they have been learned in the previous trainings. 

The final loss score for 1,458 is 1.76 (3sf).

**[Next Step]**: The model's ability to create multi-paragraphs has decreased due to the short assistant reply from the datasets. The next step is to follow ChinChilla's rule.


---

## TENTH TRAINING RECORD IS BELOW


10. According to ChinChilla, every parameter has around 20 tokens, applying this rule for 381-million parameters, the model needs around 7.6-billion tokens. Starting with 300-million tokens before gradually
building up the trained token count. The first checkpoint is that the model will aim for 1B tokens and revise with Q&A datasets to see its reasoning level. 

**(New Plan)**: The plan is save a back-up copy of the base model after every 1B checkpoint. If the model has improved, instead of shifting the internal weights from Q&A to general knowledge again, we directly utilize the base model that we saved for time efficiency.

The newly created file size has 300,000,714 tokens. The number of episodes for one completion is 18,310 (4sf).

The final loss score for 18,310 is 3.24 (3sf).

[Token Process]: 700,000,000 / 1,000,000,000 tokens

**[Next Step]**: Next step is to complete the last 300-million tokens to reach 1B tokens.


---

## ELEVENTH TRAINING RECORD IS BELOW

11. Completing the second ~300M tokens to enhance the model's world knowledge further.

[Token Process]: 1,000,000,000 / 1,000,000,000 tokens

The loss score for Episode 18,325 is 2.90 (2sf).

**[Next Step]**: Reviewing Q&A formatted datasets before testing the model's reasoning skill after training 1B tokens of general knowledge.


---

## TWELFTH TRAINING RECORD IS BELOW

12. The file contains 11,947,373 tokens. For 2 epochs, the number of episodes is 1,458 (4sf). 

The loss score for Episode 1,458 is 2.50 (2sf).

**[Next Step]**: The model's reasoning ability has improved: the model is able to politely answer every question on the point, refuse unethic and illegal queries.
However, it still hallucinate slightly later in the sentence. The result is reasonable because the loss score stopped at 2.5, meaning the model was about to lock
in on the Q&A datasets. A loss score around 1.4 to 1.6 provides the model a solid Q&A foundation. Additionally, because 'flytech/python-codes-25k' does not teach
the model to explain its generated code, I have found the datasets like 'Magicoder-Evol-Instruct-110K' and 'Evol-Instruct-Code-80k-v1', which helps users to write
and break down the definition or application of the code either during or at the end of model's text generation. So, the next step is to lower the loss score to
between 1.4 - 1.6 before proceeding. 


---

## THIRTEENTH TRAINING RECORD IS BELOW

13. Using the same file, 11,947,373 tokens, the loss score has decreased to 1.29 on the second run. Although it is below the preferred range, 1.4 to 1.6, 1.29 is
not considered as overfitting base on the model's reasoning performance.

The final loss score for 1,458 is 1.29 (3sf)

**[Next Step]**: The next step is create the new Q&A file that incorporates new coding explanations so the model gradually adapts how to explain the code rather
than just showing. 


---

## FOURTEENTH TRAINING RECORD IS BELOW

14. This generated file has 20,581,982 tokens, requiring 2,512 episodes to complete 2 epochs. 

The final loss score for Episode 2,512 is ...

**[Next Step]**: ...

