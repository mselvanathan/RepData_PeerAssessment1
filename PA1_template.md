Title
========================================================

This is an R Markdown document. Markdown is a simple formatting syntax for authoring web pages (click the **MD** toolbar button for help on Markdown).

When you click the **Knit HTML** button a web page will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:

Set Working Directory, download and unzip file.

```{r}
setwd("C:/Users/Administrator/Documents/GitHub/RepData_PeerAssessment1")
if(!file.exists("./data")){dir.create("./data")}

fileUrl <- "https://github.com/mselvanathan/RepData_PeerAssessment1/blob/master/activity.zip"

download.file(fileUrl, destfile="./data/datafile.zip")

unzip("./data/datafile.zip")

```
Read file
```{r}
a <- read.csv("./activity.csv", header=TRUE, sep=',', colClasses = c("numeric", "Date", "character"))

```
Remove NAs
```{r}
a.has.na <- apply(a, 1, function(x){any(is.na(x))})
a.filtered <- a[!a.has.na,]

```
Find sum of steps by date

```{r}
b <- tapply(a.filtered$steps, a.filtered$date, sum, simplify=FALSE)
```

Convert list 'b' to a dataframe c
```{r}
c <- data.frame( date = rep(names(b), lapply(b, length)), steps = unlist(b))

```
#draw barchart

```{r}
p <- ggplot(c, aes(x=date, y=steps)) + geom_bar(stat="identity")
p + theme(axis.title.x=element_text(size=16, lineheight=.9, family="Times", face="bold.italic", colour="red"))
p1 <- p + ggtitle("Total steps taken on each day") + 
  theme(plot.title=element_text(size=rel(1.0), lineheight=.4, 
                                family="serif", face="bold.italic", color="blue"))
p1 + theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```
#Find mean & median of steps by date
```{r}
mean.steps <- tapply(a.filtered$steps, a.filtered$date, mean, simplify=FALSE)
median.steps <- tapply(a.filtered$steps, a.filtered$date, median, simplify=FALSE)

```
#Find Missing values in dataframe a

```{r}
sapply(a, function(x) sum(is.na(x)))
```
#Replace Missing values with mean of the variable
```{r}
a$steps[is.na(a$steps)] <- mean(a$steps, na.rm = TRUE)

```







You can also embed plots, for example:

```{r fig.width=7, fig.height=6}
plot(cars)
```

