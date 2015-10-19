# Rstuff

<h4>Find median values for haplotypes based on multiple csv files with repeating values</h4>

```
temp<- list()
med<-list()
newfile<-list()
ldf <- list() # creates a list
listcsv <- dir(pattern = "*.CSV") # creates the list of all the csv files in the directory
for (k in 1:length(listcsv)){
  ldf[[k]] <- read.csv(listcsv[k], head=F, sep="\t")
  print(listcsv[[k]])
  for (i in 1:nrow(ldf[[k]])){
    newfile[[i]]=rep(ldf[[k]][i,4],ldf[[k]][i,5])
    }
temp[[k]]<- unlist(newfile)
print(median(temp[[k]]))
  }
```
