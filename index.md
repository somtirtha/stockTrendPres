---
title       : Stock Trends App
subtitle    : 
author      : Somtirtha Roy
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Introduction

- This app is an attempt to learn about how to build interactive apps using shiny.
- The app enables us to search for a stock symbol and see its trend between a data range.
- It uses widgets in order to interact with the app and does some manipulation in the backend.

---

## The App

- The app is divided in two components: the ui and the server.
- The ui has all the components that is related to the layout or in web developemnt terms the html and css.
- The server has all the components that manages the backend logic of hte application.
- We have used a reactive expression in the server side code that enables us to limit the number of requests send to the yahoo api by caching the results.
- It also keeps track if the data cached is outdated and fetches the latest data from the  api.
- This prevents sneding too many requests to the api and getting blacklisted.
- We also have a checkbox to convert the price values to log scale.


---


## The Server

- The server side code handles the stock symbol request from the client side and sends back the data from the yahoo api.




```r
library(quantmod)

shinyServer(function(input, output) {
  dataInput <- reactive({
      getSymbols(input$stockSymb, src = "yahoo", from = input$dates[1],
                 to = input$dates[2], auto.assign = FALSE)
  })
  
  output$plot <- renderPlot({data <- dataInput()
      chartSeries(data, theme = chartTheme("white", up.col='red'), 
                  type = "line", log.scale = input$log, TA = NULL)
  })
})
```

```
## Error in eval(expr, envir, enclos): could not find function "shinyServer"
```

---

## Conclusion

- This app acts as a template for further exploration of shiny
- It is a nifty app and gives scope to add more functionality for advanced interaction like projected stock prediction etc.

