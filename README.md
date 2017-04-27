# Rstuff
<h5>Tukey HSD results on ggplot</h5>

```
data$value<-data$total_num_nod_primary
data$treatment<- data$Strain
# What is the effect of the treatment on the value ?
model=lm( data$value ~ data$treatment )
ANOVA=aov(model)

# Tukey test to study each pair of treatment :
TUKEY <- TukeyHSD(x=ANOVA, 'data$treatment', conf.level=0.95)

# Tuckey test representation :
plot(TUKEY , las=1 , col="brown" )


# I need to group the treatments that are not different each other together.
generate_label_df <- function(TUKEY, variable){
  
  # Extract labels and factor levels from Tukey post-hoc 
  Tukey.levels <- TUKEY[[variable]][,4]
  Tukey.labels <- data.frame(multcompLetters(Tukey.levels)['Letters'])
  
  #I need to put the labels in the same order as in the boxplot :
  Tukey.labels$treatment=rownames(Tukey.labels)
  Tukey.labels=Tukey.labels[order(Tukey.labels$treatment) , ]
  return(Tukey.labels)
}

# Apply the function on my dataset
LABELS=generate_label_df(TUKEY , "data$treatment")


# A panel of colors to draw each group with the same color :
my_colors=c( rgb(143,199,74,maxColorValue = 255),rgb(242,104,34,maxColorValue = 255), rgb(111,145,202,maxColorValue = 255),rgb(254,188,18,maxColorValue = 255) , rgb(74,132,54,maxColorValue = 255),rgb(236,33,39,maxColorValue = 255),rgb(165,103,40,maxColorValue = 255))

# Draw the basic boxplot
a=boxplot(data$value ~ data$treatment , ylim=c(min(data$value) , 1.1*max(data$value)) , col=my_colors[as.numeric(LABELS[,1])] , ylab="Number of nodules" , xlab="Strain", main="")

# I want to write the letter over each box. Over is how high I want to write it.
over=0.1*max( a$stats[nrow(a$stats),] )

#Add the labels
text( c(1:nlevels(data$treatment)) , a$stats[nrow(a$stats),]+over , LABELS[,1]  , col=my_colors[as.numeric(LABELS[,1])] )

```

<h5>Separate different files for affymetrix</h5>

```
###splitting file
mydata<-read.table("6sample_data.txt", header=T)
mydata2<-data.frame(mydata, sep="\\")
library(reshape)
library(tidyr)
mydata3<-separate(data = mydata2, col = Probeset_ID, into = c("left", "marker","middle"), sep = "\\-")
mydata4<-subset(mydata3[,-c(1,3,10)])
spt1<-split(mydata4, mydata4$marker) 
newcolname<-c("Marker","A","B")
lapply(names(spt1), function(x){write.table((t(spt1[[x]])[-1,]), quote=FALSE, col.names=FALSE, file = paste("output", x, ".txt",sep =""))})
```

<h4>Concatenate all columns into a single column and remove NaN</h4>

```
data<-read.csv("file.csv",header=F)
c<-as.vector(data[,1])
for (i in 2:ncol(data)){
  n<-as.vector(data[,i])
  m<-c(c,n)
  c<-m
}
m<-m[ !is.nan(m)]

```

<h4>Find median depth based on multiple csv files with repeating values</h4>

```
temp<- list()
newfile<-list()
data<-list()
ldf <- list() # creates a list
listcsv <- dir(pattern = "*.csv") # creates the list of all the csv files in the directory
datab<-c("File", "Median Depth")
for (k in 1:length(listcsv)){
  ldf[[k]] <- read.csv(listcsv[k], head=F, sep="\t") #gets the list of files
  print(listcsv[[k]])
  for (i in 1:nrow(ldf[[k]])){
    newfile[[i]]=rep(ldf[[k]][i,4],ldf[[k]][i,5]) #repeats the values in col 4 according to the number assigned in col 5
  }
  temp[[k]]<- unlist(newfile) #unlist to calculate median in each file
  print(median(temp[[k]]))
  data[[k]]<-c(listcsv[[k]],median(temp[[k]])) #concatenate results
  datab<-rbind(datab,data[[k]]) #add each result to the file
}
write.csv(datab, file="Depthoutput.txt", col.names=FALSE,row.names=FALSE) #Filename is .txt to avoid confusions in reading files from same folder

```
<h4>Barplot without spaces in between plots </h4>

```
barplot(y,x,width=1, space=c(0,0),col=c(1:5))
```
