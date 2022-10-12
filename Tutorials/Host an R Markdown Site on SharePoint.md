# Create an R Markdown Site

## Create a script to rename HTML extensions to ASPX

Create a .bat file containing the file path of your _site folder and the __rename *__ keyword.
```
cd "C:\Users\e525902\Desktop\Admin Deputy Project\_site" 
rename *.html *.aspx
```
Run this .bat file every time you re-render your R Markdown site and publish.  

## Multi-page Websites
The above code to rename a single page works fine, but creating a website with a navigation bar and numerous hyperlinks embedded in the body can be slightly more complex.  Luckily, you can apply the same concept here with a few minor adjustments.

### YAML Configuration
For starters, you have to make sure all hyperlinks point to a `.aspx` file when routing to internal pages. Next, make sure to reference `.aspx` files in the `href` argument of your YAML file. 

Here is an example.
```
name: "A Multipage Webiste"
navbar:
  title: Company Dashboard
  type: inverse
  right:
    - text: "Home"
      href: index.aspx
    - text: "Products"
      href: index.aspx
      menu:
        - text: "Contact Us"
          href: index.aspx
```

## Create a SharePoint folder to hold the _site folder_
Run the batch file to rename the outputted HTML files.  Copy and paste these `.aspx` files and any subdirectories such as `www` and `site_libs` to the `_site` folder on SharePoint.  Open the `index.asp` on SharePoint and test out the navigation links to ensure they lead to the `.aspx` routes defined in your YAML configuration. 

## Host and Share the SharePoint site 
