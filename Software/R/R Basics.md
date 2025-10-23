# How to do data analysis in R (a 20 minute introduction)

This video provides a concise introduction to data analysis using R, even for those without prior programming experience.

## Getting Started with R and RStudio

* **Installation:** The video guides users on installing both R (the programming language) and RStudio (the recommended front-end interface). [00:00:26]
* **Basic Operations:** R can be used like a graphing calculator for simple arithmetic (e.g., 5+7) and functions (e.g., absolute value). [00:01:13]
* **Variables:** You can assign values to variables (e.g., `x <- -12`) and perform operations on them. [00:01:27]
* **Vectors:** R allows you to store multiple values in a single variable called a vector (e.g., `y <- c(-12, 6, 0, -1)`). Operations on vectors are typically performed component-wise. [00:01:55]

## Importing and Exploring Data

* **Importing Data:** The video demonstrates how to easily import datasets, such as Excel spreadsheets or CSV files, into RStudio using the file browser and the "Import Dataset" option. [00:02:55]
* **Packages:** R's functionality can be extended with "packages," which are add-on sets of functions. The `readxl` package is used for importing Excel files. [00:05:17]
    * You need to install a package once (`install.packages("readxl")`) and then load it in each R session you want to use it (`library(readxl)`). [00:06:03]
* **Understanding Data:**
    * The `View()` command opens a dataset in an interactive viewer. [00:04:44]
    * The `mean()` command calculates the average of a column. To specify a column, use the format `dataset_name$column_name`. [00:06:41]
    * The `na.rm = TRUE` argument in functions like `mean()` tells R to remove missing values (NAs) before calculation. [00:08:40]
    * The `glimpse()` command provides a top-level overview of your dataset, including row/column counts, variable names, and data types. [00:13:55]
    * You can get help on functions or datasets using `?function_name` or `?dataset_name`. [00:13:07]

## Data Manipulation with `dplyr` (part of Tidyverse)

* **Tidyverse:** A collection of R packages designed for data science that work well together. Key packages include `dplyr` for data manipulation and `ggplot2` for visualization. [00:10:04]
* **Filtering Rows:** The `filter()` command subsets rows based on conditions. For example, `filter(mpg, cty >= 20)` keeps rows where city mileage is 20 or more. Use `==` for equality checks. [00:15:52]
* **Adding/Modifying Columns:** The `mutate()` command adds new columns or modifies existing ones. For example, `mutate(mpg, cty_metric = cty * 0.425144)` creates a new column `cty_metric`. [00:20:02]
* **The Pipe Operator (`%>%`):** This operator (shortcut: Cmd/Ctrl + Shift + M) passes the output of one function as the first argument to the next, making code more readable. Example: `mpg %>% filter(cty >= 20) %>% mutate(...)`. [00:22:05]
* **Grouped Summaries:**
    * `group_by()` groups the data by one or more categorical variables. [00:24:25]
    * `summarize()` calculates summary statistics for each group. Example: `mpg %>% group_by(class) %>% summarize(mean_cty = mean(cty))`. [00:24:43]

## Data Visualization with `ggplot2` (part of Tidyverse)

* **Grammar of Graphics:** `ggplot2` is based on the idea that plots are built in layers. [00:27:23]
* **Basic Plotting:**
    * The `ggplot()` command initiates a plot, specifying the dataset and "aesthetics" (how variables map to visual properties). `aes(x = variable_name, y = variable_name, color = variable_name)`. [00:27:54]
    * "Geoms" specify the type of plot (e.g., `geom_histogram()`, `geom_point()` for scatter plots, `geom_freqpoly()`). [00:29:26]
    * Layers are added with a `+` sign.
* **Customization:**
    * `labs()` can be used to change axis labels and titles. [00:30:00]
    * `geom_smooth(method = "lm")` adds a linear regression line to a scatter plot. [00:31:57]
    * `scale_color_brewer(palette = "Dark2")` changes the color palette of the plot. [00:33:41]

## Communicating Results with R Markdown

* **R Markdown Documents:** A way to combine R code, its output (including plots and tables), and formatted text into a single document. [00:34:18]
* **Structure:**
    * **YAML Header:** Contains metadata like title, author, date, and output format (e.g., HTML). [00:35:05]
    * **Code Chunks:** Blocks of R code enclosed by ```{r} and ```. [00:35:27]
    * **Formatted Text:** Regular text with simple formatting options (e.g., headers with `#`, bold with `**`). [00:35:34]
* **Knitting:** The "Knit" button processes the R Markdown file, executes the R code, and generates the final output document. [00:37:04]
* **Code Chunk Options:** You can control whether the code itself, its output, or both are shown in the final document. [00:38:01]

This video emphasizes that R is a powerful tool for data analysis and encourages viewers to explore further.

Video URL: https://www.youtube.com/watch?v=yZ0bV2Afkjc