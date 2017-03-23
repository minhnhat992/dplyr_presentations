<style>
body {
    overflow: scroll;
}
</style>




Data Wrangling with R
========================================================
author: Minh Bui
date: 3/24/2017
autosize: true
font-family: "Arial"
width: 1500



So, What is Data Wrangling ?
========================================================
Manipulate your "raw" data into different forms

Examples

* Filtering/sorting data

* Calculate new columns from existing Ones

* Removing or rename empty cells/values

* Merge/join different table together

* Create Excel-like pivot tables to summarize your data



Why is it important ?
========================================================

- 80% of a data scientist's time will be spent on cleaning data


- Your boss will likely ask you to do it!



Why use R for data manipulation ?
========================================================

- Faster than Excel

- Automate the process, no more manual works!

- Huge support on data wrangling. You can manipulate date using either :
    + Base R codes
    + Packages such as [**dplyr**](https://cran.r-project.org/web/packages/dplyr/vignettes/introduction.html) 
    

So, What is dplyr ? Why you should know it ?
========================================================
- It is an [**R package**](http://www.statmethods.net/interface/packages.html) - program that someone wrote and shared to the public

- Created by Hadley Wickham

- Advantages over base R
 + Faster
 + Easier to learn and understand
 
Dataset - Diamonds
========================================================

Let's use an example of the popular Diamonds dataset.


```r
install.packages("nycflights13")
```

```r
library(ggplot2)
head(diamonds)
```

```
# A tibble: 6 × 10
  carat       cut color clarity depth table price     x     y     z
  <dbl>     <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
1  0.23     Ideal     E     SI2  61.5    55   326  3.95  3.98  2.43
2  0.21   Premium     E     SI1  59.8    61   326  3.89  3.84  2.31
3  0.23      Good     E     VS1  56.9    65   327  4.05  4.07  2.31
4  0.29   Premium     I     VS2  62.4    58   334  4.20  4.23  2.63
5  0.31      Good     J     SI2  63.3    58   335  4.34  4.35  2.75
6  0.24 Very Good     J    VVS2  62.8    57   336  3.94  3.96  2.48
```

dplyr - Single Table Verbs
========================================================
- The core components of dplyr

- Main verbs provide basic function for data manipulation
  + filter()  : Filtering data by rows
  + arrange() : Sorting
  + select()  : Choose columns
  + mutate()  : Create new columns
  + summarise() : summarize table
  
- These verbs can be used by itself or in combination!


Example - Filter Rows with filters()
========================================================
incremental: true

- filter() allows you to get a subset of rows based on different criteria 
- Diamonds with "Ideal" cut?

```r
library(dplyr)

filter(diamonds, cut == "Ideal")
```

```
# A tibble: 21,551 × 10
   carat   cut color clarity depth table price     x     y     z
   <dbl> <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
1   0.23 Ideal     E     SI2  61.5    55   326  3.95  3.98  2.43
2   0.23 Ideal     J     VS1  62.8    56   340  3.93  3.90  2.46
3   0.31 Ideal     J     SI2  62.2    54   344  4.35  4.37  2.71
4   0.30 Ideal     I     SI2  62.0    54   348  4.31  4.34  2.68
5   0.33 Ideal     I     SI2  61.8    55   403  4.49  4.51  2.78
6   0.33 Ideal     I     SI2  61.2    56   403  4.49  4.50  2.75
7   0.33 Ideal     J     SI1  61.1    56   403  4.49  4.55  2.76
8   0.23 Ideal     G     VS1  61.9    54   404  3.93  3.95  2.44
9   0.32 Ideal     I     SI1  60.9    55   404  4.45  4.48  2.72
10  0.30 Ideal     I     SI2  61.0    59   405  4.30  4.33  2.63
# ... with 21,541 more rows
```

- Diamonds with "Ideal" cut and price over 400 ?

```r
filter(diamonds, cut == "Ideal", price > 400)
```

```
# A tibble: 21,489 × 10
   carat   cut color clarity depth table price     x     y     z
   <dbl> <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
1   0.33 Ideal     I     SI2  61.8    55   403  4.49  4.51  2.78
2   0.33 Ideal     I     SI2  61.2    56   403  4.49  4.50  2.75
3   0.33 Ideal     J     SI1  61.1    56   403  4.49  4.55  2.76
4   0.23 Ideal     G     VS1  61.9    54   404  3.93  3.95  2.44
5   0.32 Ideal     I     SI1  60.9    55   404  4.45  4.48  2.72
6   0.30 Ideal     I     SI2  61.0    59   405  4.30  4.33  2.63
7   0.35 Ideal     I     VS1  60.9    57   552  4.54  4.59  2.78
8   0.30 Ideal     D     SI1  62.5    57   552  4.29  4.32  2.69
9   0.30 Ideal     D     SI1  62.1    56   552  4.30  4.33  2.68
10  0.28 Ideal     G    VVS2  61.4    56   553  4.19  4.22  2.58
# ... with 21,479 more rows
```

- Diamonds with either "Ideal" or "Premium" cut

```r
filter(diamonds, cut == "Ideal" | cut == "Premium")
```

```
# A tibble: 35,342 × 10
   carat     cut color clarity depth table price     x     y     z
   <dbl>   <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
1   0.23   Ideal     E     SI2  61.5    55   326  3.95  3.98  2.43
2   0.21 Premium     E     SI1  59.8    61   326  3.89  3.84  2.31
3   0.29 Premium     I     VS2  62.4    58   334  4.20  4.23  2.63
4   0.23   Ideal     J     VS1  62.8    56   340  3.93  3.90  2.46
5   0.22 Premium     F     SI1  60.4    61   342  3.88  3.84  2.33
6   0.31   Ideal     J     SI2  62.2    54   344  4.35  4.37  2.71
7   0.20 Premium     E     SI2  60.2    62   345  3.79  3.75  2.27
8   0.32 Premium     E      I1  60.9    58   345  4.38  4.42  2.68
9   0.30   Ideal     I     SI2  62.0    54   348  4.31  4.34  2.68
10  0.24 Premium     I     VS1  62.5    57   355  3.97  3.94  2.47
# ... with 35,332 more rows
```


Example - Sort Rows with arrange()
========================================================
incremental: true
- Use arrange() to reorders your table based on criteria

- Sort diamonds based on price 

```r
arrange(diamonds, price)
```

```
# A tibble: 53,940 × 10
   carat       cut color clarity depth table price     x     y     z
   <dbl>     <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
1   0.23     Ideal     E     SI2  61.5    55   326  3.95  3.98  2.43
2   0.21   Premium     E     SI1  59.8    61   326  3.89  3.84  2.31
3   0.23      Good     E     VS1  56.9    65   327  4.05  4.07  2.31
4   0.29   Premium     I     VS2  62.4    58   334  4.20  4.23  2.63
5   0.31      Good     J     SI2  63.3    58   335  4.34  4.35  2.75
6   0.24 Very Good     J    VVS2  62.8    57   336  3.94  3.96  2.48
7   0.24 Very Good     I    VVS1  62.3    57   336  3.95  3.98  2.47
8   0.26 Very Good     H     SI1  61.9    55   337  4.07  4.11  2.53
9   0.22      Fair     E     VS2  65.1    61   337  3.87  3.78  2.49
10  0.23 Very Good     H     VS1  59.4    61   338  4.00  4.05  2.39
# ... with 53,930 more rows
```

- Use desc() to sort descending

```r
arrange(diamonds, desc(price))
```

```
# A tibble: 53,940 × 10
   carat       cut color clarity depth table price     x     y     z
   <dbl>     <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
1   2.29   Premium     I     VS2  60.8    60 18823  8.50  8.47  5.16
2   2.00 Very Good     G     SI1  63.5    56 18818  7.90  7.97  5.04
3   1.51     Ideal     G      IF  61.7    55 18806  7.37  7.41  4.56
4   2.07     Ideal     G     SI2  62.5    55 18804  8.20  8.13  5.11
5   2.00 Very Good     H     SI1  62.8    57 18803  7.95  8.00  5.01
6   2.29   Premium     I     SI1  61.8    59 18797  8.52  8.45  5.24
7   2.04   Premium     H     SI1  58.1    60 18795  8.37  8.28  4.84
8   2.00   Premium     I     VS1  60.8    59 18795  8.13  8.02  4.91
9   1.71   Premium     F     VS2  62.3    59 18791  7.57  7.53  4.70
10  2.15     Ideal     G     SI2  62.6    54 18791  8.29  8.35  5.21
# ... with 53,930 more rows
```


Example - Select columns with select()
========================================================
incremental: true

- select() help you keep or remove columns
- Keep *carat*, *cut* and *color* columns

```r
select(diamonds, carat, cut, color)
```

```
# A tibble: 53,940 × 3
   carat       cut color
   <dbl>     <ord> <ord>
1   0.23     Ideal     E
2   0.21   Premium     E
3   0.23      Good     E
4   0.29   Premium     I
5   0.31      Good     J
6   0.24 Very Good     J
7   0.24 Very Good     I
8   0.26 Very Good     H
9   0.22      Fair     E
10  0.23 Very Good     H
# ... with 53,930 more rows
```

- Alternate way

```r
select(diamonds, carat : color)
```

```
# A tibble: 53,940 × 3
   carat       cut color
   <dbl>     <ord> <ord>
1   0.23     Ideal     E
2   0.21   Premium     E
3   0.23      Good     E
4   0.29   Premium     I
5   0.31      Good     J
6   0.24 Very Good     J
7   0.24 Very Good     I
8   0.26 Very Good     H
9   0.22      Fair     E
10  0.23 Very Good     H
# ... with 53,930 more rows
```

- Remove *carat* from table

```r
select(diamonds, -carat)
```

```
# A tibble: 53,940 × 9
         cut color clarity depth table price     x     y     z
       <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
1      Ideal     E     SI2  61.5    55   326  3.95  3.98  2.43
2    Premium     E     SI1  59.8    61   326  3.89  3.84  2.31
3       Good     E     VS1  56.9    65   327  4.05  4.07  2.31
4    Premium     I     VS2  62.4    58   334  4.20  4.23  2.63
5       Good     J     SI2  63.3    58   335  4.34  4.35  2.75
6  Very Good     J    VVS2  62.8    57   336  3.94  3.96  2.48
7  Very Good     I    VVS1  62.3    57   336  3.95  3.98  2.47
8  Very Good     H     SI1  61.9    55   337  4.07  4.11  2.53
9       Fair     E     VS2  65.1    61   337  3.87  3.78  2.49
10 Very Good     H     VS1  59.4    61   338  4.00  4.05  2.39
# ... with 53,930 more rows
```

- You can also rename columns with select()

```r
select(diamonds, new_color = color)
```

```
# A tibble: 53,940 × 1
   new_color
       <ord>
1          E
2          E
3          E
4          I
5          J
6          J
7          I
8          H
9          E
10         H
# ... with 53,930 more rows
```

- If you want to keep all of the columns, use rename() instead

```r
rename(diamonds, new_color = color)
```

```
# A tibble: 53,940 × 10
   carat       cut new_color clarity depth table price     x     y     z
   <dbl>     <ord>     <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
1   0.23     Ideal         E     SI2  61.5    55   326  3.95  3.98  2.43
2   0.21   Premium         E     SI1  59.8    61   326  3.89  3.84  2.31
3   0.23      Good         E     VS1  56.9    65   327  4.05  4.07  2.31
4   0.29   Premium         I     VS2  62.4    58   334  4.20  4.23  2.63
5   0.31      Good         J     SI2  63.3    58   335  4.34  4.35  2.75
6   0.24 Very Good         J    VVS2  62.8    57   336  3.94  3.96  2.48
7   0.24 Very Good         I    VVS1  62.3    57   336  3.95  3.98  2.47
8   0.26 Very Good         H     SI1  61.9    55   337  4.07  4.11  2.53
9   0.22      Fair         E     VS2  65.1    61   337  3.87  3.78  2.49
10  0.23 Very Good         H     VS1  59.4    61   338  4.00  4.05  2.39
# ... with 53,930 more rows
```

Example - Add new columns with mutate()
========================================================
incremental: true

- mutate() allows you to create new columns from existing ones

- What's price per carat ?

```r
mutate(diamonds, 'price per carat' = price/carat)
```


```
# A tibble: 53,940 × 6
   carat       cut color clarity price `price per carat`
   <dbl>     <ord> <ord>   <ord> <int>             <dbl>
1   0.23     Ideal     E     SI2   326          1417.391
2   0.21   Premium     E     SI1   326          1552.381
3   0.23      Good     E     VS1   327          1421.739
4   0.29   Premium     I     VS2   334          1151.724
5   0.31      Good     J     SI2   335          1080.645
6   0.24 Very Good     J    VVS2   336          1400.000
7   0.24 Very Good     I    VVS1   336          1400.000
8   0.26 Very Good     H     SI1   337          1296.154
9   0.22      Fair     E     VS2   337          1531.818
10  0.23 Very Good     H     VS1   338          1469.565
# ... with 53,930 more rows
```

Example - Summarise values with summarise()
========================================================
incremental: true
- summarise() allows you to compute statistics (averange, max , min)

- What's the average price in this table?

```r
summarise(diamonds, average_price = mean(price))
```

```
# A tibble: 1 × 1
  average_price
          <dbl>
1        3932.8
```

- You can create an Excel-like pivot table with summarise() and group_by ()

- What's the average price for each *cut* ?

```r
group_by_cut <- group_by(diamonds, cut)

summarise(group_by_cut, average_price = mean(price))
```

```
# A tibble: 5 × 2
        cut average_price
      <ord>         <dbl>
1      Fair      4358.758
2      Good      3928.864
3 Very Good      3981.760
4   Premium      4584.258
5     Ideal      3457.542
```

- How about average carat and depth for each cut ?

```r
summarise(group_by_cut, average_size = mean(carat), average_depth = mean(depth))
```

```
# A tibble: 5 × 3
        cut average_size average_depth
      <ord>        <dbl>         <dbl>
1      Fair    1.0461366      64.04168
2      Good    0.8491847      62.36588
3 Very Good    0.8063814      61.81828
4   Premium    0.8919549      61.26467
5     Ideal    0.7028370      61.70940
```

- Common functions (*sum*,*mean*, *max*, *min*, *count*, etc.)


Chainning in dplyr
========================================================
- The reason why dplyr is popular!

- Use this symbol %>%  to create a data pipelines
 + We use %>% to connect one command from each other.
 + The *output* of one command becomes the *input* for the next
 
- Make elegant and readable codes !

Examples of Chainning
========================================================
incremental: true
- What we want to do:
 + Filter "Premium" cut
 + Keep only *carat*, *cut*, *color*, *depth*, *price*
 + Create a new column, named "price_per_carat"
 
- Without chainning , your codes will look like this

```r
data <- filter(diamonds, cut == "Premium")
data <- select(data, carat, cut, color, depth, price)
data <- mutate(data, price_per_carat = price/carat )

data
```

```
# A tibble: 13,791 × 6
   carat     cut color depth price price_per_carat
   <dbl>   <ord> <ord> <dbl> <int>           <dbl>
1   0.21 Premium     E  59.8   326        1552.381
2   0.29 Premium     I  62.4   334        1151.724
3   0.22 Premium     F  60.4   342        1554.545
4   0.20 Premium     E  60.2   345        1725.000
5   0.32 Premium     E  60.9   345        1078.125
6   0.24 Premium     I  62.5   355        1479.167
7   0.29 Premium     F  62.4   403        1389.655
8   0.22 Premium     E  61.6   404        1836.364
9   0.22 Premium     D  59.3   404        1836.364
10  0.30 Premium     J  59.3   405        1350.000
# ... with 13,781 more rows
```

- Much nicer with chainning (I think...):

```r
diamonds %>% 
  filter(cut == "Premium") %>% 
  select(carat, cut, color, depth, price) %>% 
  mutate(price_per_carat = price/carat)
```

```
# A tibble: 13,791 × 6
   carat     cut color depth price price_per_carat
   <dbl>   <ord> <ord> <dbl> <int>           <dbl>
1   0.21 Premium     E  59.8   326        1552.381
2   0.29 Premium     I  62.4   334        1151.724
3   0.22 Premium     F  60.4   342        1554.545
4   0.20 Premium     E  60.2   345        1725.000
5   0.32 Premium     E  60.9   345        1078.125
6   0.24 Premium     I  62.5   355        1479.167
7   0.29 Premium     F  62.4   403        1389.655
8   0.22 Premium     E  61.6   404        1836.364
9   0.22 Premium     D  59.3   404        1836.364
10  0.30 Premium     J  59.3   405        1350.000
# ... with 13,781 more rows
```

- You can keep the chain forerver!

```r
# find the average price for each color and cut !
diamonds %>% 
  filter(cut == "Premium") %>% 
  select(carat, cut, color, depth, price) %>% 
  mutate(price_per_carat = price/carat) %>% 
  group_by(cut, color) %>% 
  summarise(average_price = mean(price))
```

```
Source: local data frame [7 x 3]
Groups: cut [?]

      cut color average_price
    <ord> <ord>         <dbl>
1 Premium     D      3631.293
2 Premium     E      3538.914
3 Premium     F      4324.890
4 Premium     G      4500.742
5 Premium     H      5216.707
6 Premium     I      5946.181
7 Premium     J      6294.592
```

Additional Information on Dplyr
========================================================

- This quick tutorial (https://cran.r-project.org/web/packages/dplyr/vignettes/introduction.html)
- [Dplyr's cheatsheet](https://www.rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf)

Thank You!
========================================================

- LinkedIn : https://www.linkedin.com/in/minhbui1/

- Source Codes for this presentation : https://github.com/minhnhat992/dplyr_presentations/tree/master

