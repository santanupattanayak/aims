## Introduction to RNNs

Recurrent Neural Networks (RNNs) are a class of neural architectures designed to model sequential or time-dependent data. Unlike feedforward networks, RNNs incorporate memory of previous inputs through cycles in the network’s structure, enabling them to capture temporal dynamics.
At their core, RNNs maintain a hidden state $`h_t`$ that is updated at each time step $`t`$ based on the current input $`x_t`$ and the previous hidden state $`h_{t-1}`$. This recursive formulation allows the network to maintain context over sequences, making them well-suited for tasks like:
Natural language processing (NLP): language modeling, machine translation, sentiment analysis
Time series forecasting: stock prediction, sensor data analysis
Sequence labeling and generation: named entity recognition, handwriting and speech generation
However, traditional RNNs struggle with long-term dependencies due to vanishing and exploding gradient problems during backpropagation through time (BPTT). To overcome this, more advanced variants like Long Short-Term Memory (LSTM) and Gated Recurrent Unit (GRU) were developed, incorporating gating mechanisms to regulate information flow.

## Mathematical Equations governing RNNs
There are two equations that govern the dynamics of a RNN
1. The hidden state update equation where hidden state $`h_t`$ that is updated at each time step $`t`$ based on the current input $`x_t`$ and the previous hidden state $`h_{t-1}`$ as below:
   
   $`h_t`$ = $`f(`$ $`W_{hh}`$$`h_{t-1}`$  + $`W_{xh}`$$`h_{t-1})`$ where

   $`h_t \in \mathbb{R}^{n}`$ : hidden state at time $`t`$ containing the information of inputs seen till time $`t`$
   
   $`x_t \in \mathbb{R}^{m}`$ : Input at time $`t`$
   
   $`W_{hh} \in \mathbb{R}^{n \times n}`$: Hidden to hidden weight recurrence matrix

   $`W_{xh} \in \mathbb{R}^{n \times m}`$: Input to hidden  weight matrix

   $`f`$ is a non linear activation function that acts elementwise on the linear transform.

2. The output at every step is based on the updated hidden state $`h_t`$ at time $`t`$ and can be expressed as

   $`y_t = g(W_{ho}h_t)`$ where

   $`W_{ho} \in \mathbb{R}^{V \times n}`$ : Hidden to output weight matrix. $`V`$ can be the size of the volcabulary or the number of classes.

   $`g`$ is a non linear activation function that produces the output. Can be a softmax of proabilities over vocabulary words or number of classes for some classifiction tasks.
   
   

   
   

## Understanding Vanishing Gradient in RNN

1. Traditional sequence models like RNNs and LSTMs suffer from loss of memory as sequences get longer.
2. This can be explained mathematically how RNNs typically work. As we have seen in the RNN formulation the state $`x_t`$ at time $`t`$ updates the hidden state $h_{t}$ from  $`x_t`$ and $h_{t-1}$ using the  recurrence  $`h_t`$ = $`f(`$ $`W_{hh}`$$`h_{t-1}`$  + $`W_{xh}`$$`h_{t-1})`$ where the sigmoid is aplied elementwise on the linear transform. The $`f`$ function is usually the sigmoid or the $`tanh`$ function.

3. Now for the RNN to learn useful correlation between words/subtokens at positions $`t`$ and $`t-k`$ esentially at distance $`k`$ during backpropagation the Jacobian of $`h_t`$ wrt to  $`h_{t-k}`$ i.e.  $`\nabla_{h_{t-k}} h_t`$ should have significant value.

4. Let's probe $`\nabla_{h_{t-k}} h_t`$ . It can be expressed by chain rule as   $`\prod_{j=t:t-k+1}\nabla_{h_{j-1}} h_j`$

5. Now $`\nabla_{h_{t-1}} h_t = f^{'}(t)W_{hh}`$ where $`f^{'}(t)`$ is a diagonal matrix housing the  derivatives for each dimension of hidden variable $`h_t`$. To make it more clear if we represent  $`\tilde{h_{t}}=`$ $`\begin{bmatrix}\tilde{h_{t}^{(0)}} & .. & \tilde{h_{t}^{(i)}} & ..\end{bmatrix}^{T} =`$   $`W_{hh}`$$`h_{t-1}`$  + $`W_{xh}`$$`h_{t-1}`$  then $`f^{'}(t)`$ in its $`i`$th diagonal entry would house $`\frac{\partial h_{t}^{(i)}}{\partial \tilde{h_{t}^{(i)}}}`$

6. Now that we have laid out the required maths and notations the  Jacobian $`\nabla_{h_{t-k}} h_t`$ can be expressed as
   
   $`\nabla_{h_{t-k}} h_t`$  = $`\prod_{j=t:t-k+1}\nabla_{h_{j-1}} h_j=`$ $`f^{'}(t)W_{hh}*`$$`f^{'}(t-1)W_{hh}*..*`$$`f^{'}(t-k+1)W_{hh}`$

7. Since the terms $`f^{'}(.)`$ are diagnal matrices we can use the commutative property to come up with the Jacobian as below

    $`\nabla_{h_{t-k}} h_t`$  = $`\prod_{j=t:t-k+1}\nabla_{h_{j-1}} h_j=`$ $`f^{'}(t)*`$$`f^{'}(t-1)*..*`$$`f^{'}(t-k+1)*W_{hh}^{k}`$

8. The hidden to hidden matrix $`W_{hh}`$ is a square and in general a non symmetrical matrix. And hence we can decompose
   $`W_{hh} = USU^{-}`$ where $`U`$ is matrix of the eigen vectors of $`W_{hh}`$ while $`S`$ is the diagonal matrix having the eigen values of $`W_{hh}`$. Substituting the eigen value decomposition in (7) we get the final form of the Jacobian as

   $`\nabla_{h_{t-k}} h_t`$  = $`\prod_{j=t:t-k+1}\nabla_{h_{j-1}} h_j=`$ $`f^{'}(t)*`$$`f^{'}(t-1)*..*`$$`f^{'}(t-k+1)*US^{k}U^{-1}`$  

9. When the magnitude of the eigen values $`|\lambda_{i}|`$ of $`W_{hh}`$ housed in $`S`$ are less than 1 then  $`|\lambda_{i}|^{k} \rightarrow 0 `$ when $`k`$ is large. Likewise the eigen values with magnitude > 1 with explode for large $`k`$. Vanishing gradient happens when the largest of the eigen value $`max_{i}|\lambda_{i}| < 1`$ as then $`S^{k} \rightarrow 0`$ . The $`f^{'}(t)`$ won't be able to help much in avoiding the vanishing gradient in this case as the derivatives of the sigmoids are themselves less than 1. 

   

