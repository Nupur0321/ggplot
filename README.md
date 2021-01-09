# ggplot
Visualisation in R by ggplot

```
install.packages(“ggplot2”)
library(ggplot2)
```

x=c(1,2,3,4,5,6)\
a=c(2,4,3,5,6,7) \
b=c(7,6,5,4,3,2)\ 
c=c(10,15,11,10,10,10)\
data2=data.frame(time=x, a1=a, b1=b, c1=c)\
View(data2)

# plot 1
ggplot(data=data2, aes(x=time, y=a1))+geom_line()
# adding a second line 
ggplot(data=data2)+geom_line(aes(x=time, y=a1))+ geom_line(aes(x=time, y=b1))
# graph with colours and adjusted y axis
ggplot(data=data2)+geom_line(aes(x=time, y=a1), colour="red")+ geom_line(aes(x=time, y=b1), colour="blue")+ scale_y_continuous(name="something", limits=c(-2,10))
# tidy data 
#install.packages(“tidyverse”)
library(tidyverse)

#ggplot generally prefers “long” data to “wide” data – can you figure out what the below code is doing?
data3=gather(data2, canditate, n, -time)\
View(data3)\
# all equivalent to data3
data4=gather(data2, canditate, n, -1) \
data5=gather(data2, canditate, n, c(a1,b1,c1)) \
data6=gather(data2, canditate, n, 2:4)

# plot with legend
ggplot(data=data3)+geom_line(aes(x=time,y=n,colour=canditate))\

# plot with legend and manually selected colours and a title
ggplot(data=data3)+geom_line(aes(x=time, y=n, colour=canditate))+scale_color_manual(values=c("#FF0000", "#E69F00", "#56B4E9"))+ ggtitle("This is the title of my plot")\

# check out the following plotly command
p1 = ggplot(data=data3)+geom_line(aes(x=time, y=n, colour=canditate))+scale_color_manual(values=c("#FF0000", "#E69F00", "#56B4E9"))+ ggtitle("This is the title of my plot")\

```
install.packages(“plotly”)
library(plotly)
ggplotly(p1)
```

# expand the plot to a separate window and investigate it!

# read the below vignette for tidy-data; check out the associated paper (link provided in vignette) 
vignette("tidy-data")

#run the below demo for colors 
demo("colors")
# do an internet search for a full list of colour options available – particularly the names of colours and how to call them


#.......................................................\
GGplot2 and dplyr – A further example: Execute the following codes

Part 1 – Basic Plots\
# Install the following packages:
# Installation install.packages('ggplot2') 
# Loading library(ggplot2)
# Load the data
data(mtcars)\
df <- mtcars[, c("mpg", "cyl", "wt")]\
# Convert cyl to a factor variable 
df$cyl <- as.factor(df$cyl)\
head(df)

# Basic scatter plot
ggplot(data = mtcars, aes(x = wt, y = mpg)) + geom_point()\

# Change the point size, and shape
ggplot(mtcars, aes(x = wt, y = mpg)) + geom_point(size = 2, shape = 23)\

# Examine the documentation for the qplot() function
# Generate a basic scatter plot
qplot(x = mpg, y = wt, data = df, geom = "point")\
# Scatter plot with smoothed line
qplot(mpg, wt, data = df, geom = c("point", "smooth"))\
# Scatter plot with different point types for the different cyl values 
qplot(mpg, wt, data = df, colour = cyl, shape = cyl)\


Part2 boxplot\

# set random number seed – this will ensure we all get the same random numbers
set.seed(1234)\
# generate random data frame
wdata = data.frame( sex = factor(rep(c("F", "M"), each=200)), weight = c(rnorm(200, 55), rnorm(200, 58)))\

head(wdata)

# Basic box plot from data frame
qplot(sex, weight, data = wdata, geom= "boxplot", fill = sex)\
# Violin plot 
qplot(sex, weight, data = wdata, geom = "violin")\
# Dot plot 
qplot(sex, weight, data = wdata, geom = "dotplot", stackdir = "center", binaxis = "y", dotsize = 0.5)\

#.............Desnisty Graphs\
# Use geometry function
ggplot(wdata, aes(x = weight)) + geom_density() \
# OR use stat function
ggplot(wdata, aes(x = weight)) + stat_density()\

# Define a as the meta-plot of wdata
a <- ggplot(wdata, aes(x = weight))\

# Basic area plot
a + geom_area(stat = "bin")\
# change fill colors by sex
a + geom_area(aes(fill = sex), stat ="bin", alpha=0.6) + theme_classic()\
# Basic density plot
a + geom_density()\
# change line colors by sex
a + geom_density(aes(color = sex))\


# Change fill color by sex
# Use semi-transparent fill: alpha = 0.4
a + geom_density(aes(fill = sex), alpha=0.4)\
# Add mean line and Change color manually
#(you need to make 2 changes to this code before it will work)
a + geom_density(aes(color = sex)) + geom_vline(data=mu,aes(xintercept=group.mean, color=sex), linetype="dashed") + scale_color_manual(values=c("#999999", "#E69F00"))\


# Basic frequency polygon plot
a + geom_freqpoly()\
# change y axis to density value and change theme
a + geom_freqpoly(aes(y = ..density..)) + theme_minimal()\
# change color and linetype by sex
a + geom_freqpoly(aes(color = sex, linetype = sex)) + theme_minimal()\


# Basic histogram
a + geom_histogram()\
# change line colors by sex
a + geom_histogram(aes(color = sex), fill = "white", position = "dodge")\
