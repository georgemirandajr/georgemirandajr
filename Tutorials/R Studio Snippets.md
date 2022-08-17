Snippets are fantastic for repetitive or hard-to-remember chunks of code.  R Studio code snippets give you the ability to quickly repurpose your code as templates.  You're probably using snippets already because R Studio comes packaged with tons of them (e.g., `library`, `apply` family of functions, `if`).
I use snippets primarily for inserting custom functions, grouping and summarizing data with dplyr, building get requests, and much more.

## Enable Snippets
Go to Tools > Global Options > Code.
Check the box under the *Snippets* section.  This enables code snippets in R Studio.
Go to Edit Snippets to create and modify code snippets.

You'll notice that there's already tons of snippets for each type of file.  Since I want to write a snippet for R code, I'll select the R files tab, but there's also Markdown, Javascript, HTML, and others. 

## Create a Snippet
I'm going to show you how to create an R snippet for a custom function called createProject, which takes a directory path and a string to create a custom project folder.

Start by using the snippet keyword followed by the name of your snippet. Later, when you're coding and start typing this name in your script, you'll see this snippet suggested to you. 

Next, decide if there are any parts of the snippet you want to be changeable by the user.  Typically, you would want to turn a function's arguments into this type of field.  Use the `${1:name}` syntax wherever you want these fields.  The number reflects the order a user can "tab" through and should be incremented.  The name is a hint you give to the user letting them know what to enter.

In this example, taken from a [previous tutorial](https://github.com/georgemirandajr/georgemirandajr/blob/main/Tutorials/Create%20a%20Custom%20Project%20Template.md), I want the function to show up with 2 arguments presented to me.  I want the `choose.dir()` to always be there, but I want to have the ability to change the name parameter.  I'll hard-code the `choose.dir()` and use the `${}` syntax to indicate this is a user-input field.  
  
Save your changes and exit.  
  
### Example: A Custom Function
```
snippet createProject
	createProject(path = choose.dir(), ${1:name})
```  

### Example: read_fwf
`readr` is a fantastic package that contains functions to read data from just about any format.  My organization uses fixed-width-files and `readr` contains a function called `read_fwf` that makes it easy to read this type of file.  Because the fixed widths are custom to each file, I keep a reference table of the field names and field widths in a data.frame and use this along with `read_fwf` to import the data.  Here's an example of a snippet that makes this easy for me to put those pieces together.  I prefer to use data.table, so I can customize this always import as a `data.table` using `setDT`.

```
snippet fwf
setDT(readr::read_fwf( ${1:fname},
                         progress = FALSE,
                         col_types = paste(rep('c', length( ${2:fieldName} ) ), collapse = ''),
                         fwf_positions( ${3:Start},
                                        ${4:End},
                                        col_names = ${5:fieldName}) ) )
```

## Use Snippets

Create a new script or just start typing the name of your snippet in the console.  R Studio will suggest snippets, functions, and data while you type.  Find your snippet, hit enter, and voila!  If for some reason it doesn't show up as you type, you can just type your snippet name, hit `tab` on your keyboard, and the code snippet will appear. 

Then tab through the snippet to enter the parameters (i.e., arguments) that you supplied when you created your snippet.  

This is a simple example, but you can make them more complex with more arguments or defaults that you prefer.  I use snippets to call on custom functions or code patterns that I have trouble remembering.

Code away!  
