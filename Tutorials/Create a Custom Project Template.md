## Custom Project Folders
I am going to write a function that will create a directory with all the sub-directories needed for a data analysis project.  

## Motivation
All data analysts work on the same thing: data.  However, the way we organize that work varies based on individual preferences, work environments, and experience.  When I start a big project, I need to have a predictable framework that makes it quick and easy to start working while keeping my code streamlined.
  
## My Custom Project Folder
My projects need to have the following sub-directories:
- `/data` to keep any input data for my code. This can be static reference files or manually updated spreadsheets, etc.  Bascially, any data that feeds into my analysis.
- `/output` to store output files (e.g., .csv, .xlsx, .txt, .docx, .pptx, .pdf).  These are datasets that I can share with my boss or external parties.
- `/code` to store the various R files needed for the analysis, including a global.R file to call the other scripts in this folder.
- `/utils` to store log.txt files, .bat, or helper files.  Not always needed, but they'll go here when needed.
- `/www` to store images such as logos or css files to style reports that utilize the output data.

By having my work organized this way, I can easily start an Rmarkdown report file in the project root, switch to a shiny app, or generate a static html website.  I never know where the project might be headed, so this structure works for me in nearly all cases.  
  
## The Code
  
```
createProject = function( path, projectName, ... ) {

  # Create a folder for the project
  dir.create( paste0( path, "/", projectName ) )

  # Create the sub-folders
  subFolderNames = c( 'data', 'output', 'code', 'utils', 'www' )
  subFolderNames = path.expand( paste0( path, "/", projectName, "/", subFolderNames ) )
  sapply( subFolderNames, dir.create )

}
```

## How to Use

I prefer to use `choose.dir()` in the `path` argument and simply give it my project name rather than type out the whole path.
  
```
createProject( choose.dir(), "Genius Project" )
```

This will prompt me to choose a folder in which to create my new project and use the string "Genius Project" as the folder name.
  
