## R Studio Addins Changed My Life

Want to take your coding efficiency into hyper-drive?  R Add-Ins will do that!  I have used some addins in the past, but the power of addins really hit me when I started writing them for my own team.  By packaging my code as addins, the team can take advantage of the tools I've built that make the job more efficient.  It's also useful for those who are starting to learn R because as an addin author, you can abstract the code as much as you need.  

## What is an R Studio Addin?

This seldomly discussed, yet powerful feature of R Studio allows you to take advantage of extensions written for R by simply clicking "Addins" and choosing from a list of installed addins.  **Addins are distributed as R packages**, which means all you have to do is write your code, create a package, publish it, and let others know that they can download it.  

## Examples

There are tons of addin examples. Here are a few of my favorites.
- [esquisse](https://www.dreamrs.fr/) to quickly visualize your data and save the code to reproduce it
- [questionr](https://juba.github.io/questionr/) to do survey analysis
- [ViewPipeSteps](https://github.com/daranzolin/ViewPipeSteps) to understand the output of each step in a dplyr workflow (also helps debugging)

You can create an addin as simple or as complex as you want.  It can be something simple like this example from R Studio's documentation page.
```
insertInAddin <- function() {
  rstudioapi::insertText(" %in% ")
}
```
The code above inserts the `%in%` operator.  You can map this addin to a keyboard shortcut to make things even more efficient, which is similar to how the pipe `%>%` operator works when you use `Ctrl+Shift+M`.  But if you can make a [shiny gadget](https://shiny.rstudio.com/articles/gadgets.html), then you can do even more fun things like the [`questionr`](https://juba.github.io/questionr/) addin. 

## Build Your Own

To build your own make sure to have `rstudioapi` installed.  This package allows you to create and configure your addin.  

Let's build off a [previous tutorial](https://github.com/georgemirandajr/georgemirandajr/blob/main/Tutorials/Create%20a%20Custom%20Project%20Template.md) where we wrote a function that created a custom project folder for us; however, this time we will turn this function into an addin.  The main difference is that the original function must be called by typing something like `createProject(path=choose.dir(), "new project name")`, whereas the addin can be selected from a menu.  Taking it from a useful function to an even more convenient addin takes just a few steps.

### 1. Create a package
Start by creating a new project by clicking on the New Project icon.  Creating a project sounds like a lot, but I promise it's not.  
![image](https://user-images.githubusercontent.com/6701264/172499540-5011dcfd-cf32-4124-96af-5395018e2c6b.png)

Choose New Directory 
![new-project-min](https://user-images.githubusercontent.com/6701264/172500413-b6497e8a-4bdc-4f51-b898-a742145c30f2.png)

Select R Package
![project-type-min](https://user-images.githubusercontent.com/6701264/172500419-18954533-1e5e-4351-9e8b-5e3f5550b524.png)

Name Your R Package
Your package can contain many addins and functions, even data and Rmarkdown templates, but generally you should keep each package focused on one theme.  
![create-project-min](https://user-images.githubusercontent.com/6701264/172500425-f40de58b-97c8-41ae-8534-38a8cc170a79.png)

### 2. Write your function(s)
Here is the function that will create a custom project folder.  This was originally created for [previous tutorial](https://github.com/georgemirandajr/georgemirandajr/blob/main/Tutorials/Create%20a%20Custom%20Project%20Template.md). 

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
Again, this function is great, but to use it we must remember the function name and arguments.  It may not sound so hard to remember these things, but over time you are likely to become overwhelmed as you build many useful tools for yourself and colleagues.  Also, distributing this function is not easy.  Without packaging it and publishing it somewhere, like Github or CRAN, you would have to share the file via shared drives or email.  

### 3. Add some templates

Another great feature of addins is that you can add files in your package that can be used by your functions.  This means that you can share datasets and templates!  
In this example, I'll add some template files that I like to include in every project: `global.R` and an R markdown document called `Report_Template.Rmd`.  The `global.R` file calls on my frequently used R packages, establishes variables to connect to my commonly used datasets, and calls on other scripts that will actually perform the analysis. The `Report_Template.Rmd` file is a vetted template for my output.  You can add any files you want to include in your custom project folder, I'm just using these as examples.  To add template files, create a folder in your package path at `inst/extdata` and copy your files there.

### 4. Modify the function to copy my custom templates
Since we want our addin to use these templates, we need to modify our original function by adding some code that copies/pastes the templates into the user's new project folder.

```
# save to R/createProject.R
createProject <- function(path, projectName, ...) {

# ... continued from above ...

  # Copy the report template
  f = system.file("extdata", "Report_Template.Rmd", package = "YOUR_PACKAGE_NAME" )  # replace the package argument with the actual name of your package

  if ( nchar(f) > 0 ) {
    file.copy( from = f, to = paste0( path, "/", projectName) )  # copies the template file stored in extdata into the user's new project folder
  }

  # Copy the global.R file
  g = system.file("extdata", "global.R", package = "raddish" )

  if ( nchar(g) > 0 ) {
    file.copy( from = g, to = paste0( path, "/", projectName, "/code"))
  }
```

### 5. Make the Shiny UI
Now we get to make the function interact with our user.  To do this, we'll create a new file that will contain a function that creates the mini shiny app.
The basic structure of our UI will take the following shape.  Notice that my new function is called `new_analysis`.
```
new_analysis <- function() {  // this is the 'big' function that will launch the mini shiny app 
  createProject <- function(path, projectName, ...) {
    ...all the code to create a template project folder...
  }
  
  ui <- miniUI::miniPage( ) // the ui code
  server <- function(input, output, session) { } // the server code (calls the createProject())
  
  viewer <- shiny::dialogViewer("New Analysis Project") // choose where the app runs - other options include browserViewer() and paneViewer()
  shiny::runGadget(ui, server, viewer = viewer)  // run the shiny app on the viewer 
}
```

Filling in this skeleton looks like this.  Notice the explicit calls to external packages.
```
require(shiny)
require(miniUI)

new_analysis <- function() {
  createProject <- function(path, projectName, ...) {

    # Create a folder for the project
    dir.create( paste0( path, "/", projectName) )

    # Create the sub-folders
    subFolderNames = c('data', 'output', 'code', 'utils', 'www')
    subFolderNames = path.expand( paste0( path, "/", projectName, "/", subFolderNames) )
    sapply( subFolderNames, dir.create )

    # Copy the report template
    f = system.file("extdata", "Report_Template.Rmd", package = "YourPackageName" )

    if ( nchar(f) > 0 ) {
      file.copy( from = f, to = paste0( path, "/", projectName) )
    }

    # Copy the global.R file
    g = system.file("extdata", "global.R", package = "YourPackageName" )

    if ( nchar(g) > 0 ) {
      file.copy( from = g, to = paste0( path, "/", projectName, "/code"))
    }

  }

  ui <- miniUI::miniPage(
    miniUI::gadgetTitleBar("Create a Custom Project Folder"),
    miniUI::miniContentPanel(
      ## Name the new project
      shiny::textInput("folder_name_input", "New Project Name"),

      # Choose where to create the new project
      shiny::helpText("Choose a folder create your project in"),
      shiny::actionButton("choose_dir", "Choose folder"),

      # Change to this directory?
      shiny::checkboxInput("switch_dir", "Switch to this folder"),

      # Show the selected path to the new project
      shiny::uiOutput("created_your_folder")
    )
  )

  server <- function(input, output, session) {

    # Provide a default directory
    dir_path = shiny::reactiveValues( path= getwd() )

    # Update the selected path
    shiny::observeEvent( input$choose_dir, {
      dir_path$path = choose.dir()
      dir_path$path = gsub("\\\\", "/", dir_path$path)
    })

    ## Your reactive logic goes here.
    shiny::observeEvent( input$folder_name_input, {

      output$created_your_folder <- shiny::renderUI({
        p(
          paste0(
            "Creating ", dir_path[['path']], "/", input$folder_name_input
          )
        )
      })

    } )

    # Listen for the 'done' event. This event will be fired when a user
    # is finished interacting with your application, and clicks the 'done'
    # button.
    shiny::observeEvent(input$done, {

      # Here is where your Shiny application might now go and affect the
      # objects open in RStudio or trigger side-effects (i.e., create a project folder).
      createProject( dir_path[['path']], input$folder_name_input )

      if ( input$switch_dir ) {
        setwd( paste0( dir_path[['path']], "/", input$folder_name_input ) )
      }

      # At the end, your application should call 'stopApp()' here, to ensure that
      # the gadget is closed after 'done' is clicked.
      shiny::stopApp()
    })
  }

  # We'll use a dialog viwer
  viewer <- shiny::dialogViewer("New Analysis Project")

  shiny::runGadget(ui, server, viewer = viewer)

}

# Call the addin
# new_analysis()  // only run this line when developing. Make sure to comment-out otherwise you'll have trouble building the package.

```

### 6. Register the addin
You need to register your addin by creating a file in your package folder at `inst/rstudio/addins.dcf`.  It takes just 4 lines (example shown below) to register an addin.  Do this for each addin that you want to include and show to the user.

```
Name: Create New Analysis  // the name displayed in the drop-down list of addins
Description: Creates a custom project folder for a new analysis  // displayed as help text to describe the addin for your user
Binding: new_analysis  // the name of the function you are adding in
Interactive: true  // see below
```

Setting `Interactive` to `true` means that we want R Studio to make use of `shiny` and `miniUI` by opening a dialog box that allows the user to choose a directory and give the custom project folder a name.  If we set `interactive` to `false`, then we would leave our function as-is and let it be called manually like in the [previous tutorial](https://github.com/georgemirandajr/georgemirandajr/blob/main/Tutorials/Create%20a%20Custom%20Project%20Template.md). 

### 7. Finish up your R package
Document your package and make sure dependencies are included in your DESCRIPTION file.  I recommend using `roxygen2` package, which has tools to assist you with documentation and testing.

Once everything is documented and **tested**, you can build the package using Clean and Rebuild.
![buildpkg](https://user-images.githubusercontent.com/6701264/184960639-d2d18493-39d9-4b29-9424-18cf34de8046.png)

### 8. Publish your R package
You can publish your package on Github or CRAN.  Github is great for smaller projects, especially if the audience is small.  If your addins could serve the greater R community, publish your package on CRAN.  Make sure to follow CRAN's guidelines.  

Publishing with Github is easy, especially if you have Github Desktop.  Add a local repository (i.e., the folder containing your package).  Commit and then push to Github.  Use Github Desktop to update Github whenever you make changes.  Others can then download the package from Github using something like `devtools::install_github('yourGithubName/projectFolder')`

Publishin on CRAN takes a little longer, but it has its advantages.  
1. The package goes through a rigorous vetting process (fully document and test your package prior to submitting to CRAN)
2. It will be widely available to anyone using R.  

For a step-by-step on how to publish to CRAN, visit https://r-pkgs.org/release.html

## Conclusion
Publishing an addin is simple and can make your code development more efficient.  In turn, addins can make your life easier, as well as that of anyone who needs to use your code.  As an education tool, addins are capable of supporting R learners by exposing them to code and making it easier to see results. 
