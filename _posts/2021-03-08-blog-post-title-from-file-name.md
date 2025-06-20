## Understanding Attention

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

9. When the magnitude of the eigen values $`|\lambda|`$ of $`W_{hh}`$ housed in $`S`$ are less than 1 then  $`|\lambda|^{k} \rightarrow 0 `$ when $`k`$ is large.

   

    
Enter attention: a method that allows the model to assign relevance weights to different parts of an input when producing an output.
Think of it like reading a novel and highlighting the lines that resonate with you. The attention mechanism does something similar—it dynamically weighs words, pixels, or tokens based on their contextual importance.
This is a sample blog post. You can talk about all sorts of fun things here.

---

### This is a header

#### Some T-SQL Code

```tsql
SELECT This, [Is], A, Code, Block -- Using SSMS style syntax highlighting
    , REVERSE('abc')
FROM dbo.SomeTable s
    CROSS JOIN dbo.OtherTable o;
```

#### Some PowerShell Code

```powershell
Write-Host "This is a powershell Code block";

# There are many other languages you can use, but the style has to be loaded first

ForEach ($thing in $things) {
    Write-Output "It highlights it using the GitHub style"
}
```
