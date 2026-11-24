**It is recommended to look through the 'Model Initializations' folder before reading this explanation, because the Euclidean code uses 'self.model_parameters'
(a 1-dimensional list), which is not explained in this file.**

> Applying Euclidean Normalization, personally, is a great approach to prevent exponential gradient increase during backpropagation. In the past, I have
> encountered a lot of exploding gradient problems. For the model's architecture, Euclidean Normalization does not 100% solve this issue, the exploding gradient
> occurred because I did not find the maximum number for every row and subtract every scalar by the highest value in a matrix (softmax) - this is to stabilize
> every scalar to prevent the numbers growing up as the complex matrix multiplication moves on. 

## 1. E.N. calculation method is to square every weight's gradient before adding them together - this only calculate for one layer: Σg^2(l, i), where'l' represents
this specific layer and 'i' means the number of gradients (starting from i = 0). Calculating every neural layer's gradient and add them up, before taking the square
root to find the final direction of the global gradient. 
```python
euclid_norm = t.sqrt(sum(t.sum(gradient.grad ** 2) for gradient in self.model_parameters if gradient.grad is not None))
```

## 2. The if-statement checks whether the global gradient across all the neural layers has exceeded the custom value, which is set as 1.0. If it is,
loop through every matrix in 'self.model_parameters' (containing all the weights for the model) and check if there is a computed gradient. When the
second if-statement is true, the math calculates the new gradient value that will be below the custom number (1.0). In other words, it preserves the
same gradient direction but changes its magnitude, meaning if the output is '75.0', the math stabilizes the gradient by using 'gradient.grad *= (1.0 / (euclid_norm + 1e-6))',
preventing the optimizer (or Adaptive Moment Estimation) from applying massive, unsteady value to the model's weights. Additionally, '1e-6' (1 x 10^-6) represents
the epilson value - it is not always a constant, depending on what we are trying to calculate - this avoids division-by-zero if "euclid_norm" becomes 0.0, adding that tiny
number does not produce noticeable affect on the gradient result.

> Note: If "euclid_norm" is 0.0 in rare case, then (1.0 / (1 x 10^-6)) equals 1,000,000. 1 million is a massive number, and you definitely think this would break the model's
> weights. However, the catch is that when the norm is 0.0, every gradient element is also 0.0. They cancel out the huge value and produce the result as 0.0 (0 * 1,000,000).  
```python
if euclid_norm > 1.0:
for gradient in self.model_parameters:
  if gradient.grad is not None:
    gradient.grad *= (1.0 / (euclid_norm + 1e-6))
```

## (FULL-SECTION CODE BELOW)
```python
euclid_norm = t.sqrt(sum(t.sum(gradient.grad ** 2) for gradient in self.model_parameters if gradient.grad is not None))
if euclid_norm > 1.0:
  for gradient in self.model_parameters:
    if gradient.grad is not None:
      gradient.grad *= (1.0 / (euclid_norm + 1e-6))
```
