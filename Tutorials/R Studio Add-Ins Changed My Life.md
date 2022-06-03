## R Studio Addins Changed My Life

Want to take your coding efficiency into hyper-drive?  R Add-Ins will do that!  The power of addins really hit me when I started writing my own for the team that I work with.  By packaging my code as addins, my team can now take advantage of the tools I've built that make the job more efficient.  

## What is an R Studio Addin?

This seldomly discussed, yet powerful feature of R Studio allows you take advantage of extensions written for R by simply clicking "Addins" and choosing from a list of installed addins.  An addin is distributed through R packages, which means all you have to do is write your code, create a package, publish it, and let others know that they can download it.  

## Examples

There are tons of addin examples
- [esquisse](https://www.dreamrs.fr/) to quickly visualize your data and save the code to reproduce it
- [questionr](https://juba.github.io/questionr/) to do survey analysis
- [ViewPipeSteps](https://github.com/daranzolin/ViewPipeSteps) to understand the output of each step in a dplyr workflow (also helps debugging)

You can create an addin as simple or as complex as you want.  It can be something simple like this
```
insertInAddin <- function() {
  rstudioapi::insertText(" %in% ")
}
```
The code above inserts the `%in%` operator.  You can map this addin to a keyboard shortcut to make things even more efficient, which is similar to how the pipe `%>%` operator works when you use `Ctrl+Shift+M`.  But if you can make a shiny gadget, then you can do even more fun things. 

## Build Your Own

