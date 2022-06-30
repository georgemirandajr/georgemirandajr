## R Studio Addins Changed My Life

Want to take your coding efficiency into hyper-drive?  R Add-Ins will do that!  The power of addins really hit me when I started writing them for my own team.  By packaging my code as addins, the team can take advantage of the tools I've built that make the job more efficient.  

## What is an R Studio Addin?

This seldomly discussed, yet powerful feature of R Studio allows you take advantage of extensions written for R by simply clicking "Addins" and choosing from a list of installed addins.  **Addins are distributed as R packages**, which means all you have to do is write your code, create a package, publish it, and let others know that they can download it.  

## Examples

There are tons of addin examples. Here are a few of my favorites.
- [esquisse](https://www.dreamrs.fr/) to quickly visualize your data and save the code to reproduce it
- [questionr](https://juba.github.io/questionr/) to do survey analysis
- [ViewPipeSteps](https://github.com/daranzolin/ViewPipeSteps) to understand the output of each step in a dplyr workflow (also helps debugging)

You can create an addin as simple or as complex as you want.  It can be something simple like this
```
insertInAddin <- function() {
  rstudioapi::insertText(" %in% ")
}
```
The code above inserts the `%in%` operator.  You can map this addin to a keyboard shortcut to make things even more efficient, which is similar to how the pipe `%>%` operator works when you use `Ctrl+Shift+M`.  But if you can make a shiny gadget, then you can do even more fun things like the `questionr` addin. 

## Build Your Own

Make sure to have `rstudioapi` installed.  This package allows you to create and configure your addin.  

Let's build off another tutorial [link here](https://github.com/georgemirandajr/georgemirandajr/edit/main/Tutorials) where we wrote a function that created a custom project folder for us; however, this time we will turn this function into an addin.  The main difference is that the original function must be called by typing something like `createProject(path=choose.dir(), "new project name")`, whereas the addin can be selected from a menu of options.  Taking it from a simple function to a useful addin takes just a few steps.

### 1. Create a package
Start by creating a new project by clicking on the New Project icon.
![image](https://user-images.githubusercontent.com/6701264/172499540-5011dcfd-cf32-4124-96af-5395018e2c6b.png)

Choose New Directory 
![new-project-min](https://user-images.githubusercontent.com/6701264/172500413-b6497e8a-4bdc-4f51-b898-a742145c30f2.png)

Select R Package
![project-type-min](https://user-images.githubusercontent.com/6701264/172500419-18954533-1e5e-4351-9e8b-5e3f5550b524.png)

Name Your R Package
Your package can contain many addins and functions, even data and Rmarkdown templates, but generally you should keep each package focused on one theme.  
![create-project-min](https://user-images.githubusercontent.com/6701264/172500425-f40de58b-97c8-41ae-8534-38a8cc170a79.png)

### 2. Write your function(s)
Here is the function that will create a custom project folder.  This was originally created for previous [tutorial](www.google.com). 

```
# save to R/createProject.R
createProject <- function(path, projectName, ...) {

  # Create a folder for the project in the given path
  dir.create( paste0( path, "/", projectName) )

  # Create the sub-folders to organize my project files.
  subFolderNames = c('data', 'output', 'code', 'utils', 'www')
  subFolderNames = path.expand( paste0( path, "/", projectName, "/", subFolderNames) )
  sapply( subFolderNames, dir.create )
}
```
Again, this function is great, but to use it we must remember the function name and arguments.  It may not sound so hard to remember these things, but over time you are likely to become overwhelmed as you build many useful tools for yourself and colleagues.  Also, distributing this function is not easy.  Without packaging it and publishing it somewhere, like Github, you would have to share the file via shared drives or email.  

### 3. Add some templates

Another great feature of addins is that you can add files in your package that can be used by your functions.  This means that you can share data and templates!  

In this example, I'll add some template files that I like to include in every project: `global.R` and an R markdown document called `Report_Template.Rmd`.  The `global.R` file calls on my frequently used R packages, establishes variables to connect to my commonly used datasets, and calls on other scripts that will actually perform the analysis. The `Report_Template.Rmd` file is a vetted template for my output.  You can add any files you want to include in your custom project folder, I'm just using these as examples.  To add template files, create a folder in your package path at `inst/extdata` and copy your files there.

### 4. Modify the function to copy the templates
Since we're using an addin and our addin uses these templates, we can modify our original function by adding some code that copies/pastes the templates into the user's new project folder.

```
# save to R/createProject.R
createProject <- function(path, projectName, ...) {

# ... continued from above ...

  # Copy the report template
  f = system.file("extdata", "Report_Template.Rmd", package = "YOUR_PACKAGE_NAME" )  # replace the package argument with the actual name of your package

  if ( nchar(f) > 0 ) {
    file.copy( from = f, to = paste0( path, "/", projectName) )
  }

  # Copy the global.R file
  g = system.file("extdata", "global.R", package = "raddish" )

  if ( nchar(g) > 0 ) {
    file.copy( from = g, to = paste0( path, "/", projectName, "/code"))
  }
```

### 5. Register the addin
You need to register your addin by creating a file in your package folder at `inst/rstudio/addins.dcf`.  It takes just 4 lines to register an addin.  Do this for each addin that you want to include.

```
Name: Create Project  // the name displayed in the drop-down list of addins
Description: Creates a custom project folder  // displayed as help text to describe the addin for your user
Binding: createProject  // the name of the function you are adding in
Interactive: true  // see below
```

Setting `Interactive` to `true` means that we want R Studio to make use of `shiny` and `miniUI` by opening a dialog box that allows the user to choose a directory and give the custom project folder a name.  If we set `interactive` to `false`, then we would leave our function as-is and let it be called manually like in the previous [tutorial](www.google.com). 

### 6. Make the Shiny UI
Now we get to make the function interact with our user.  To do this, we'll modify our function again, this time to make a mini shiny application.

### 7. Finish up your R package
Document your package and make sure dependencies are included.

### 8. Publish your R package
You can publish your package on github or CRAN.  Github is great for smaller projects, especially if the audience is small.  If your addins could serve the greater R community, publish your package on CRAN.  Make sure to follow CRAN's guidelines.  
