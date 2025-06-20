## Introduction to RNNs

Recurrent Neural Networks (RNNs) are a class of neural architectures designed to model sequential or time-dependent data. Unlike feedforward networks, RNNs incorporate memory of previous inputs through cycles in the network’s structure, enabling them to capture temporal dynamics.
At their core, RNNs maintain a hidden state $`h_t`$ that is updated at each time step $`t`$ based on the current input $`x_t`$ and the previous hidden state $`h_{t-1}`$. This recursive formulation allows the network to maintain context over sequences, making them well-suited for tasks like:
Natural language processing (NLP): language modeling, machine translation, sentiment analysis
Time series forecasting: stock prediction, sensor data analysis
Sequence labeling and generation: named entity recognition, handwriting and speech generation
However, traditional RNNs struggle with long-term dependencies due to vanishing and exploding gradient problems during backpropagation through time (BPTT). To overcome this, more advanced variants like Long Short-Term Memory (LSTM) and Gated Recurrent Unit (GRU) were developed, incorporating gating mechanisms to regulate information flow.
## Understanding Vanishing Gradient in RNN

1. Traditional sequence models like RNNs and LSTMs suffer from loss of memory as sequences get longer.
2. This can be explained mathematically how RNNs typically wok. The state $`x_t`$ at time $`t`$ updates the hidden state $h_{t}$ from  $`x_t`$ and $h_{t-1}$ using the  recurrence  $`h_t`$ = $`\sigma(`$ $`W_{hh}`$$`h_{t-1}`$  + $`W_{xh}`$$`h_{t-1})`$ where the sigmoid is aplied elementwise on the linear transform.

3. Now for the RNN to learn useful correlation between words/subtokens at positions $`t`$ and $`t-k`$ esentially at distance $`k`$ during backpropagation the Jacobian of $`h_t`$ wrt to  $`h_{t-k}`$ i.e.  $`\nabla_{h_{t-k}} h_t`$ should have significant value.

4. Let's probe $`\nabla_{h_{t-k}} h_t`$ . It can be expressed by chain rule as   $`\prod_{j=t:t-k+1}\nabla_{h_{j-1}} h_j`$

5. Now $`\nabla_{h_{t-1}} h_t = \sigma^{'}(t)W_{hh}`$ where $`\sigma^{'}(t)`$ is a diagonal matrix housing the  derivatives for each dimension of hidden variable $`h_t`$. To make it more clear if we represent  $`\tilde{h_{t}}=`$ $`\begin{bmatrix}\tilde{h_{t}^{(0)}} & .. & \tilde{h_{t}^{(i)}} & ..\end{bmatrix}^{T} =`$   $`W_{hh}`$$`h_{t-1}`$  + $`W_{xh}`$$`h_{t-1}`$  then $`\sigma^{'}(t)`$ in its $`i`$th diagonal entry would house $`\frac{\partial h_{t}^{(i)}}{\partial \tilde{h_{t}^{(i)}}}`$

6. Now that we have laid out the required maths and notations the  Jacobian $`\nabla_{h_{t-k}} h_t`$ can be expressed as
   
   $`\nabla_{h_{t-k}} h_t`$  = $`\prod_{j=t:t-k+1}\nabla_{h_{j-1}} h_j=`$ $`\sigma^{'}(t)W_{hh}*`$$`\sigma^{'}(t-1)W_{hh}*..*`$$`\sigma^{'}(t-k+1)W_{hh}`$

7. Since the terms $`\sigma^{'}(.)`$ are diagnal matrices we can use the commutative property to come up with the Jacobian as below

    $`\nabla_{h_{t-k}} h_t`$  = $`\prod_{j=t:t-k+1}\nabla_{h_{j-1}} h_j=`$ $`\sigma^{'}(t)*`$$`\sigma^{'}(t-1)*..*`$$`\sigma^{'}(t-k+1)*W_{hh}^{k}`$

8. The hidden to hidden matrix $`W_{hh}`$ is a square and in general a non symmetrical matrix. And hence we can decompose
   $`W_{hh} = USU^{-}`$ where $`U`$ is matrix of the eigen vectors of $`W_{hh}`$ while $`S`$ is the diagonal matrix having the eigen values of $`W_{hh}`$. Substituting the eigen value decomposition in (7) we get the final form of the Jacobian as

   $`\nabla_{h_{t-k}} h_t`$  = $`\prod_{j=t:t-k+1}\nabla_{h_{j-1}} h_j=`$ $`\sigma^{'}(t)*`$$`\sigma^{'}(t-1)*..*`$$`\sigma^{'}(t-k+1)*US^{k}U^{-1}`$  

9. When the magnitude of the eigen values $`|\lambda_{i}|`$ of $`W_{hh}`$ housed in $`S`$ are less than 1 then  $`|\lambda_{i}|^{k} \rightarrow 0 `$ when $`k`$ is large. Likewise the eigen values with magnitude > 1 with explode for large $`k`$. Vanishing gradient happens when the largest of the eigen value $`max_{i}|\lambda_{i}| < 1`$ as then $`S^{k} \rightarrow 0`$ . The $`\sigma^{'}(t)`$ won't be able to help much in avoiding the vanishing gradient in this case as the derivatives of the sigmoids are themselves less than 1. 

   

