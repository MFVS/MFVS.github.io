# __R Shiny__

[![Shiny](https://img.shields.io/badge/Shiny-006BA1?style=for-the-badge&logo=RStudio&logoColor=white)](https://shiny.rstudio.com/)

```r
library(shiny)

ui <- fluidPage(
  titlePanel("My Shiny App"),
  sidebarLayout(
    sidebarPanel(
      uiOutput("output")
    ),
    mainPanel()
  )
)

server <- function(input, output, session) {
  output$output <- renderUI({
    query <- parseQueryString(session$clientData$url_search) # (1)!
    if ("stagUserTicket" %in% names(query)) {
      HTML(paste("stagUserTicket: ", query$stagUserTicket))
    } else {
      HTML('<a href="https://ws.ujep.cz/ws/login?originalURL=http://localhost:6305/">
      Přihlašte se pomocí STAGu
      </a>') # (2)!
    }
  })
}

shinyApp(ui = ui, server = server)
```

1. Získání query parametrů z URL adresy.
2. Odkaz na přihlášení pomocí STAGu.
