# Rstuff

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
