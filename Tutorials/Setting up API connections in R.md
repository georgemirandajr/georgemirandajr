# Setting up API connections in R

The main part of this tutorial will focus on creating a function to enable easy GET requests.  We'll take the common structure of a GET request and wrap it in a function, which can be used as an addin or snippet.

## A Basic Structure
```
# Base URL
url = ''

# Host (website domain)
hostName = ''

# Usually a public key (not encoded)
api_key = ''

# Secret key is usually encoded
secretKey = ''

res = httr::GET( url,
                 add_headers( Authorization = paste( 'Basic', secretKey ),
                              Host = hostName ),
                 query = list(
                   key = api_key, q = '',
                 ),
                 accept_json(),
                 content_type_json()
)

# rawToChar converts javascript objects (JSON) characters
api_data = fromJSON(
  rawToChar(
    res$content
  )
)
```

## Writing a Function

Some text here

## Example
Get some city information from [GeoDB Cities](https://rapidapi.com/wirefreethought/api/geodb-cities/).
```
url = "https://wft-geo-db.p.rapidapi.com/v1/geo/cities"

response = httr::VERB( "GET", 
                        url, 
                        add_headers(
                          'X-RapidAPI-Key' = 'SIGN-UP-FOR-KEY', 
                          'X-RapidAPI-Host' = 'wft-geo-db.p.rapidapi.com'
                        ), 
                        content_type( "application/octet-stream" )
                        )
                  
content(response, "text")
```

## Paginated Results
Sometimes the results you get from an API call will be paginated.  Typically, the API documentation will let you know when to expect this and explains some parameters that you can set to work with pagination.  But how do you programmtically turn each page and compile the results into a nice data.frame?  You can usually find that the API will have a field that displays a link to the next page.  Basically, you just need to create a loop that takes your initial API address, gets the results, finds the field with the next link, and repeats until the value in that field is blank (i.e., no next page).  Here's the framework for doing so.

```
```
