<img src="./man/figures/logo.png" align = "right"/>

# cicerone 

[![Travis build status](https://travis-ci.org/JohnCoene/cicerone.svg?branch=master)](https://travis-ci.org/JohnCoene/cicerone)

Provides an easy API to create guided tours for Shiny applications using [driver.js](https://kamranahmed.info/driver.js/).

## Installation

You can install {cicerone} from Github with:

``` r
# install.packages("remotes")
remotes::install_github("JohnCoene/cicerone")
```

## Usage

Let's create a very basic Shiny app to demonstrate: it takes a text input and on hitting a button simply prints it.

```r
library(shiny)

ui <- fluidPage(
  textInput("text", "Enter some text"),
  actionButton("submit", "Submit text"),
  verbatimTextOutput("print")
)

server <- function(input, output){
  txt <- eventReactive(input$submit, {
    input$text
  })

  output$print <- renderPrint(txt())
}

shinyApp(ui, server)
```

Now we can create a a guide to walk the user through the application: simply initialise a new guide from the `Cicerone` object then add `steps`.

```r
library(cicerone)

guide <- Cicerone$
  new()$ 
  step(
    el = "text",
    title = "Text Input",
    description = "This is where you enter the text you want to print."
  )$
  step(
    "submit",
    "Send the Text",
    "Send the text to the server for printing"
  )
```

This is our guide created, we can now include it the Shiny app we created earlier and start the guide. Note to that you need to include `use_cicerone` in your UI.

```r
library(shiny)

ui <- fluidPage(
  use_cicerone(), # include dependencies
  textInput("text", "Enter some text"),
  actionButton("submit", "Submit text"),
  verbatimTextOutput("print")
)

server <- function(input, output){

  # initialise then start the guide
  guide$init()$start()

  txt <- eventReactive(input$submit, {
    input$text
  })

  output$print <- renderPrint(txt())
}

shinyApp(ui, server)
```

![](./man/figures/demo.gif)

All the options are detailed in the documentation of the object: `?Cicerone`. See the [post](https://blog.john-coene.com/posts/2019-11-20-cicerone/) for more information and some rambling on the logo and package name.
