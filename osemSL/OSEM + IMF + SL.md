---
dg-publish: true
---
### 1. Data Acquisition

Choosing a number of indicators from the IMF database which we think is necessary to run the model.

Data inspection script:
```r
load(imf.data)
library(tidyverse)

# Using imf.data package to load a given dataset
test <- imf.data::load_datasets("ISF")

# Using the above dataset to call the get_series function and format the data
testdata <- test$get_series(
  ref_area = "LK",
  freq = "A"
) %>%
  pivot_longer(-TIME_PERIOD) %>%
  left_join(test$dimensions$indicator %>%
              rename(name = Value) %>%
              mutate(name = paste0("A.LK.",name)))

# Do further data filtering
final_data <- testdata %>%
  # Choose some filters here
  filter(name %in% paste0("A.LK.",c("NGDP_R_XDC", "PCPI_IX", "LUR_PT", "BCA_NGDPD"))) %>%
  
  drop_na(value) %>%
  mutate(time = as.numeric(TIME_PERIOD),
         value = as.numeric(value))

# Plotting the data
ggplot(final_data, aes(x = time, y = value, color = name)) +
geom_line() +
theme(legend.position = "none") +
facet_wrap(~ name, scales = "free_y")
```
### 2. Creating the dictionary

The indicators we picked above should be inserted into a dictionary. Following is an example of this operation. 
```r
tibble(
    model_varname = "TestVar", # this is free to choose but must be unique
    full_name = "Test Variable",
    database  = "imf",
    dataset_id = "IFS",
    freq = "A",
    ref_area = "LK",
    indicator = "NFIAXD_XDC",
    unit_measure = "USD") -> dict_imf_test
```

Better to create a function for this- to append rows to this dictionary:
```r
# Function to create a new dictionary entry and append it
create_dict_entry <- function(dict, model_varname, full_name, database = "imf", dataset_id = "IFS", freq = "A", ref_area = "LK", indicator, unit_measure) {
  # Create the new entry as a tibble
  new_entry <- tibble(
    model_varname = model_varname,
    full_name = full_name,
    database = database,
    dataset_id = dataset_id,
    freq = freq,
    ref_area = ref_area,
    indicator = indicator,
    unit_measure = unit_measure
  )
  
  # Append the new entry to the existing dictionary
  dict <- bind_rows(dict, new_entry)
  
  return(dict)
}

# Example usage: Add a new entry to the dictionary
dict_imf <- create_dict_entry(
  dict_imf, 
  model_varname = "NGDP_XDC", 
  full_name = "Nominal GDP (Constant USD)", 
  indicator = "NGDP_R_XDC", 
  unit_measure = "USD"
)

dict_imf <- create_imf_dict_entry(
  dict_imf, 
  model_varname = "PCPI_IX", 
  full_name = "Consumer Price Index", 
  indicator = "PCPI_IX", 
  unit_measure = "Index"
)

# View the updated dictionary
print(dict_imf)
```

It's still a bit tedious. There's more room for optimization. One such convenient approach is to draft a header file with the data we want and create the dictionary dynamically using that file. Then it'll add more convenience and customizability for this operation.
### 3. Running the Model

Now we can use the above dictionary to run the model:
```r
library(conflicted)
library(tidyverse)
library(osem)
conflicted::conflicts_prefer(dplyr::filter)

spec_imf <- dplyr::tibble(
  type = c(
    "n"
  ),
  dependent = c(
    "TestVar"
  ),
  independent = c(
    "indepVariable"
  )
)

model <- run_model(specification = spec_imf,
                   dictionary = dict_imf_test,
                   present = FALSE,
                   quiet = TRUE,
                   constrain.to.minimum.sample = FALSE)
```

Should understand what's going on under the hood.

### 4. World bank data integration

Map the world bank data to the existing eurostat data - whatever we can collect.
R packages:
- https://cran.r-project.org/web/packages/wbstats/vignettes/wbstats.html
- https://cran.r-project.org/web/packages/wbstats/vignettes/wbstats.html

Build a module to acquire world bank data.
Create a OSEM compatible dictionary
Run a test model using world bank data using a test specification

