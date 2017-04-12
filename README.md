# Rstuff
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
