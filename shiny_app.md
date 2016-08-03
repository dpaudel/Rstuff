ui.R
```
shinyUI(fluidPage(
  
  titlePanel("Random variable summary"),
  
  sidebarLayout(
    sidebarPanel(h1("Input"),
                 numericInput("mean_distribution",
                              label=h3("Mean of the distribution"),
                              value=1),
                 numericInput("sd_distribution", 
                              label=h3("Standard Deviation"),
                              value=0.5),
                 numericInput("number_points",
                              label=h3("Number of observations"),
                              value=100),
                 selectInput("hist_color",
                             label=h3("Color of the histogram"),
                             choices=list(
                               "Red" = "red",
                               "Orange" = "orange",
                               "Green" = "green",
                               "Light Blue" = "lightblue"),
                             selected = "red")
                 ),
    mainPanel(h1("Output"),
              textOutput("input_mean"),
              textOutput("drawn_mean"),
              plotOutput("plot_mean"))
  )
))
```

server.R
```
shinyServer(function(input, output){
  
  r_norm <- reactive({rnorm(input$number_points,
                  mean=input$mean_distribution,
                  sd=input$sd_distribution)
  })
  
output$input_mean <- renderText(
  paste("The mean of the distribution is ", input$mean_distribution
))

output$drawn_mean <- renderText(
  paste("The mean from the random distribution is ", mean(r_norm()))
)

output$plot_mean <- renderPlot(
  hist(r_norm(), col=input$hist_color)
)
})
```
