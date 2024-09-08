# Image

## Adaptive Filter

### Explanation of Terms: 
 
- **$f(x, y)$** : The original pixel value at location $$(x, y)$$.
 
- **$\hat{f}(x, y)$** : The estimated (filtered) pixel value after applying the adaptive filter.
 
- **$g(x, y)$** : The arithmetic mean value in the local neighborhood $$S_{xy}$$ (window centered at $$(x, y)$$).
 
- **$\sigma_n^2$** : Noise variance, which may be known or estimated for the image.
 
- **$\sigma_L^2$** : Local variance in the neighborhood around the pixel $$(x, y)$$.

### Behavior of the Adaptive Local Noise Reduction Filter: 
 
1. **$\sigma_n^2 = 0$**  (Zero-noise case): 
  - This represents a **trivial, noise-free situation** .
 
  - In this case, the pixel value is not altered by the filter:
$$
 \hat{f}(x, y) = g(x, y) = f(x, y) 
$$

  - Since there is no noise, the filter doesn't modify the original pixel value, as there is no need for smoothing or noise reduction.
 
2. **$\sigma_L^2 \gg \sigma_n^2$**  (Preserving edges): 
  - When the local variance $$\sigma_L^2$$ is much larger than the noise variance $$\sigma_n^2$$, this typically occurs in regions with **edges or high-intensity changes** .
 
  - In this case, the filter tries to preserve the original pixel values as much as possible:
$$
 \hat{f}(x, y) \approx g(x, y) 
$$

  - The filter recognizes the presence of an edge and avoids excessive smoothing to prevent blurring, thus preserving the edge details.
 
3. **$\sigma_L^2 \approx \sigma_n^2$**  (Smooth areas with noise): 
  - When the local variance $$\sigma_L^2$$ is approximately equal to the noise variance $$\sigma_n^2$$, this indicates that the region $$S_{xy}$$ is a **smooth area**  affected by noise.
 
  - In this case, the filter replaces the pixel value with the arithmetic mean of the neighborhood, effectively reducing noise:
$$
 \hat{f}(x, y) = \text{mean value in} \ S_{xy} 
$$

  - This is because, in smooth regions, the variation in intensity is small and mostly due to noise. Averaging the neighborhood values helps reduce this noise.

### Summary of Behavior: 
 
- **No Noise** : The filter leaves the image unchanged.
 
- **High Variance (Edges)** : The filter preserves edges by minimizing smoothing.
 
- **Low Variance (Smooth Regions)** : The filter averages the pixel values to reduce noise, as the region behaves similarly to the overall noisy image.

### Key Points: 
 
- The adaptive local noise reduction filter **adapts to local characteristics**  (variance) to either reduce noise or preserve details such as edges.
 
- In **flat regions** , where the variance is similar to noise, it reduces noise by averaging.
 
- In **edge regions** , where the local variance is high, it avoids smoothing to preserve important details.

You: 

f^​(x,y)

meaning of it?

ChatGPT: 

The symbol $$\hat{f}(x, y)$$ in the context of the **adaptive local noise reduction filter**  refers to the **estimated (filtered) pixel value**  at the coordinates $$(x, y)$$ after applying the filter.
### Meaning in Context: 
 
- The purpose of the adaptive filter is to **reduce noise**  while **preserving important image details**  (such as edges).
 
- $$f(x, y)$$ is the original pixel value, and after applying the filter, the result is $$\hat{f}(x, y)$$, which is the new (filtered) pixel value.
 
- The symbol $$\hat{f}(x, y)$$ represents the **filtered result** , meaning that the pixel value has been modified to account for the noise level in the surrounding area (neighborhood).
Thus, $$\hat{f}(x, y)$$ is the **final pixel value**  after the adaptive noise reduction filter has processed the image, adjusted based on local noise characteristics.


### Expression of the Adaptive Local Noise Reduction Filter: 
$$
 \hat{f}(x, y) = g(x, y) - \frac{\sigma_n^2}{\sigma_L^2} [g(x, y) - m_L] 
$$

Where:
 
- **$\hat{f}(x, y)$** : The filtered pixel value.
 
- **$g(x, y)$** : The original pixel value in the noisy image.
 
- **$\sigma_n^2$** : The variance of the noise, which is either known or estimated.
 
- **$\sigma_L^2$** : The local variance in the region $$S_{xy}$$, i.e., the variance in the local neighborhood around pixel $$(x, y)$$.
 
- **$m_L$** : The local mean in the region $$S_{xy}$$.

### How It Works: 
 
- The filter reduces noise by subtracting a weighted portion of the difference between the original pixel value and the local mean $$m_L$$, with the weight based on the ratio of noise variance $$\sigma_n^2$$ and local variance $$\sigma_L^2$$.
 
- If the local variance $$\sigma_L^2$$ is large compared to the noise variance $$\sigma_n^2$$, the correction term becomes small, and the pixel value is preserved (e.g., in edges or detailed areas).
 
- If the local variance $$\sigma_L^2$$ is small (indicating a flat, noisy region), the filter adjusts the pixel value closer to the local mean, effectively smoothing the noise.

### Key Points from the Image: 
 
1. **Only Noise Variance Needs to Be Known or Estimated** : 
  - The filter requires only the **The filter requires only the noise variance $\sigma_n^2$** , which can either be known beforehand or estimated from the image.
 
2. **Behavior of the Filter** : 
  - **When $\sigma_L^2 \geq \sigma_n^2$** : 
    - In regions where the local variance is greater than or equal to the noise variance, the filtered pixel value $$\hat{f}(x, y) \geq 0$$.

    - This ensures that the filtering does not introduce negative values, and the result remains a valid pixel intensity.
 
  - **When $\sigma_L^2 < \sigma_n^2$** : 
    - In regions where the local variance is less than the noise variance, the filtered pixel value might be negative, depending on how the filter is applied.
 
    - The filter provides two schemes to handle such situations:
 
      - **Scheme 1** : Set $$\sigma_L^2 = \sigma_n^2$$, so that $$\hat{f}(x, y) = m_L$$. This scheme forces the filtered value to be the local mean, which may reduce fine details but ensures that the output remains non-negative.
 
      - **Scheme 2** : Allow negative values first and then rescale the final image to bring all pixel values into a valid range (0-255, for example). However, this can reduce the dynamic range of the image.

### Summary: 
 
- The **adaptive local noise reduction filter**  adjusts the amount of smoothing based on the local variance. 
  - **In regions with high variance**  (edges or textures), the filter preserves details.
 
  - **In regions with low variance**  (flat areas affected by noise), the filter smooths the pixel values by adjusting them towards the local mean.

- The image also explains how to handle cases where the local variance is smaller than the noise variance, either by setting it to the noise variance or by allowing negative values and rescaling.

## Adaptive Median Filter 
The image you provided shows the algorithm for **adaptive median filtering**  with two key decision levels (A and B) to process each pixel in a subimage, denoted $$S_{xy}$$, with a growing window size. Here's how the algorithm works:**Steps of the Adaptive Median Filter Algorithm** :**Level 0** : 
- Set the initial window size for the subimage $$S_{xy}$$ centered at a pixel at position $$(x, y)$$. Start with a small window (e.g., 3x3).
**Level 1** : 
- Calculate three values for the subimage: 
  - $$Z_{\min}$$: Minimum pixel value in the window
 
  - $$Z_{\max}$$: Maximum pixel value in the window
 
  - $$Z_{\text{med}}$$: Median pixel value in the window
**Level A** : 
1. **Check the condition** :
$$
 Z_{\min} < Z_{\text{med}} < Z_{\max} 
$$
 
  - If true, move to **Level B**  (indicated by the green box).
 
  - If false, increase the window size (if within the maximum allowable window size $$S_{\max}$$) and repeat **Level 1** . If the window reaches $$S_{\max}$$ and the condition is still false, output $$Z_{\text{med}}$$.
**Level B** : 
1. **Check the condition** :
$$
 Z_{\min} < Z_{xy} < Z_{\max} 
$$
 
  - If true, output the pixel value $$Z_{xy}$$ (the original value at the center of the window).
 
  - If false, output the median value $$Z_{\text{med}}$$.
**Summary** : 
- The goal of **adaptive median filtering**  is to remove impulse noise (salt-and-pepper noise) while preserving edges.

- The algorithm adjusts the window size dynamically to adapt to local features of the image. It prevents noise from dominating the process by carefully comparing the median and pixel values at each step.

