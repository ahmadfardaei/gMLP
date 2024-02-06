## Introduction
[Paper](https://arxiv.org/pdf/2105.08050v2.pdf)


**Gated Multilayer Perceptron (gMLP)** is an ***attention-free*** architecture that combines the  **MLPs** with graph convolutional networks (**GCNs**) to capture long-range dependencies in data by aggregating information across multiple **tokens** or features.

The core component of gMLP is the **spatial gating unit (SGU)**, which captures long-range dependencies between tokens.

## How Does It Work?
### First section: Feed-forward Neural Network

**Step one: Converting raw input to input embeddings**

This happens using the same input and output protocols of BERT (for NLP tasks) and Vision Transformer (for computer vision tasks)
* The words are converted to word embeddings, and images are turned into patches, then patch embeddings.   


**Step two: Normalizing the embedded input**

**Step Three: Channel projection followed by an activation layer**

 * First perform linear projection along the channel axis on the inputs: $XU$
 * Then we pass the result to an activation function such as ***GeLU***: $Z = σ(XU)$
-----------------------------------------------------------

### Second part: Adding the Spatial Gated Unit (SGU)

It consists of two linear projections, followed by a gating function to control the flow of information between the two projections.

This gating mechanism allows gMLP to *selectively focus on relevant information* while suppressing less important details, leading to improved performance on tasks that require global context understanding.

**Step four: Input of SGU**

$Z$–*the output of activation functiona and the input of SGU*– is splitted into two independent parts, $Z_1$ and $Z_2$

*  $Z_2$ goes through some modifications:
> * **Normalise** $Z_2$ to improve the stability of our model.

* To allow cross-token interactions, this layer must have a contraction operation across the spatial dimension
> * A linear projection is the most straightforward choice: $f_{W,b}(Z_2) = WZ_2 + b$

> > > * Unlike self-attention, where $W$  is created dynamically from $Z$,  the spatial projection matrix $W$ in this case is independent of the input representations.


**Step five: Output of SGU**

* First, calculate the element-wise multiplication of $Z_1$ and the modified $Z_2$ as $s(Z)=Z_1 \odot f_{W,b}(Z_2)$

* Then, take $s(Z)$ through another linear projection along the channel axis: $Y = s(Z)V$

-----------------------------------------------------------
### Final part: Add the input, repeat the block for $L$ times
At the end, we add the embedded input into $Y$ and for better performance, we can repeat this block for $L$ times


![Alt text for the image]([images](https://github.com/ahmadfardaei/gMLP/blob/main/images/gMLP.png)https://github.com/ahmadfardaei/gMLP/blob/main/images/gMLP.png)
