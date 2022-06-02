Snippets are fantastic for repetitive or hard-to-remember chunks of code.  R Studio code snippets give you the ability to quickly repurpose your code as templates.  You're probably using snippets already because R Studio comes packaged with tons of them.
I use snippets primarily for inserting custom functions, grouping and summarizing data with dplyr, building get requests, and much more.

## Enable Snippets
Go to Tools > Global Options > Code.
Check the box under the *Snippets* section.  This enables code snippets in R Studio.
Go to Edit Snippets to create and modify code snippets.

You'll notice that there's already tons of snippets for each type of file.  Since I want to write a snippet for R code, I'll select the R files tab, but there's also Markdown, Javascript, HTML, and others. 

## Create a Snippet
I'm going to show you how to create an R snippet for a custom function called createProject, which takes a directory path and a string to create a custom project folder.

Start by using the snippet keyword followed by the name of your snippet. Later, when you're coding and start typing this name in your script, you'll see this snippet suggested to you. 
Back to creating the snippet - **hit enter** and write the code that you want to appear when you select this snippet.  In this example, I want the function to show up with 2 arguments presented to me.  I want the `choose.dir()` to always be there, but I want to have the ability to change the name parameter.
  
Save your changes and exit.  
  
```
snippet createProject
	createProject(path = choose.dir(), ${1:name})
```  

## Use Snippets

Create a new script or just start typing the name of your snippet in the console.  R Studio will suggest snippets, functions, and data while you type.  Find your snippet, hit enter, and voila!

You can tab through the snippet to enter the parameters (i.e., arguments) that you supplied when you created your snippet.  

This is a simple example, but you can make them more complex with more arguments or defaults that you prefer.  I use snippets to call on custom functions or code patterns that I have trouble remembering.

Code away!  
