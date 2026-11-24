**It is recommended to look through the 'Autoregressive Generation' folder before reading this explanation.**

**This is a custom next-token selector by using 'torch.topk()', where 'topk' chooses the high probabilities and extract their corresponding index. 
The second argument is to pass in the number of high-likely words so the model has more variety when responding to a user's question. Although it is
definitely possible to code 'topk' from-scratch, I use it to quickly test if the entire model works as expected. Later in the code, when the probabilities
are added together, the function, 'TokenProbabilitySelector', checks which probability reaches or exceeds the target number. When it occurs, that
specific token is the candidate.**

**1.** 'row_of_probable_words' returns a 1-dimensional (or 1D) list that extracts last row from the first batch. This is how most text-language models
determine their next token for its response. Passing 'top_k', which is 3 in this case (for testing purpose), to automatically let the function extracts 
top 3 high-likely words (probabilities) and return their index.
```python
highest_probable_next_token, token_indices = t.topk(row_of_probable_words, top_k)
```

**2.** Because 'row_of_probable_words' are previously converted from unnormalized logits - containing negatives and positives - to percentages (represented in decimals)
that is now ranged from 0.0 to 1.0, taking out the most 3 high-likely words require another percentage calculation. To calculate the new probability for each of the 3 words, we
use every float to divide by their total sum. For example, '[0.41, 0.18, 0.74]', without calculating the new percentage, it result 41%, 18%, and 74%. Because the total sum of 
those three numbers does not equal to 100%, we divide each by their total sum: (0.41 + 0.18 + 0.74) = 1.33 (total), (0.41 / 1.33) = ~31%, (0.18 / 1.33) = ~13%, and
(0.74 / 1.33) = ~56%. Note that numbers with high possibility previously still maintain with a large number. 
```python
top_tokens_percentages = highest_probable_next_token / t.sum(highest_probable_next_token)
```

**3.** Simply speaking, 'torch.rand()' randomly generates a number between 0.0 to 1.0. The output for "chosen_range" might be 'tensor([0.3817], device='cuda:0')'. The
function, 'item()', is to remove the tensor wrapper and only keep the float number - the terminal returns '0.3817'.
```python
chosen_range = t.rand(1, device = 'cuda').item()
```

**4.** "percentage_accumulation" is a variable that stores the addition of each percentage. We cannot use "token_indices" because it stores the exact positions of each
chosen percentage. Using 'enumerate' to create new indices for those three tokens, which will be '[0, 1, 2]', corresponding to [0.31, 0.13, 0.56], where Index 0 is 0.31
and so on. Because "word_percentage" still have the tensor data type, using 'item()' to acquire the pure float number and add it to "percentage_accumation". The if-statement
checks whether the currently added probability is equal or has exceeded "chosen_range". If it is, the code returns the final index from "token_indices" by using "token_id".
If any unexpected error or calculation happens, the code returns the last index in "token_indices".
```python
percentage_accumulation = 0.0
  
for token_id, word_percentage in enumerate(top_tokens_percentages):
  percentage_accumulation += word_percentage.item()
  if chosen_range <= percentage_accumulation:
    return token_indices[token_id].item()

return token_indices[-1].item()
```

**(FULL-SECTION CODE BELOW)**
```python
def TokenProbabilitySelector(self, row_of_probable_words, top_k = 3):
  highest_probable_next_token, token_indices = t.topk(row_of_probable_words, top_k)
  
  top_tokens_percentages = highest_probable_next_token / t.sum(highest_probable_next_token)
  chosen_range = t.rand(1, device = 'cuda').item()
  percentage_accumulation = 0.0
  
  for token_id, word_percentage in enumerate(top_tokens_percentages):
    percentage_accumulation += word_percentage.item()
    if chosen_range <= percentage_accumulation:
      return token_indices[token_id].item()
  
  return token_indices[-1].item()
```
