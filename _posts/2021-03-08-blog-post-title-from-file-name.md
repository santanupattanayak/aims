## Understanding Attention

Traditional sequence models like RNNs and LSTMs suffer from loss of memory as sequences get longer.
This can be explained mathematically how RNNs typically wok. The state $`x_t`$ at time `t`  
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
