### Intro
R package designed by Moritz Schwarz. 
It implements an **open-source macroeconomic model** based on the Norwegian Aggregate Model and structured as a system of interlinked equations.

### Setting up:
First R and a suited IDE (preferably RStudio) should be installed.

Then, we have to install `devtools` and `writexl` in R: 
```r
install.packages("devtools", "writexl")
```

Then we have to install the `osem` package: `devtools` has a `install_github` function which can install packages directly from github.
```r
devtools::install_github("moritzpschwarz/osem", ref='imf')
```

Once installed, to work with `osem`, we have to load the package in to the R environment:
```r
library(osem)
```

### Going through the model:

First, we define a specification table (called a tibble) listing modules:
- `type`: `n` for estimated equations and `d` for definitions/identities
- `dependent`: response variable
- `independent`: regressors or defining expressions

In the example, spec has 9 modules:
'n' equations: 
- Import
- Private Consumption
- Capital Formation
- Emissions
- Sectoral value-added sharing independent variables (Gov, Manuf, Constr, Wholesale Trade)
'd' identity:
- GDP (sum of value-added components)

| type | dependent               | independent                                                                                                                                                                            |
| ---- | ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| n    | Import                  | FinConsExpHH + GCapitalForm                                                                                                                                                            |
| n    | FinConsExpHH            |                                                                                                                                                                                        |
| n    | GCapitalForm            | FinConsExpGov + FinConsExpHH                                                                                                                                                           |
| n    | Emissions               | GDP + Export + GValueAddIndus                                                                                                                                                          |
| d    | GDP                     | GValueAddGov + GValueAddAgri + GValueAddIndus + GValueAddConstr + GValueAddWholesaletrade + GValueAddInfocom + GValueAddFinance + GValueAddRealest + GValueAddResearch + GValueAddArts |
| n    | GValueAddGov            | FinConsExpGov                                                                                                                                                                          |
| n    | GValueAddManuf          | Export + LabCostManuf                                                                                                                                                                  |
| n    | GValueAddConstr         | LabCostConstr + BuildingPermits                                                                                                                                                        |
| n    | GValueAddWholesaletrade | Export + LabCostService                                                                                                                                                                |

### Adding a dataset to the dictionary

Link: https://www.moritzschwarz.org/osem/articles/new_variable_to_dict.html

We can use the existing `dict` dictionary and add a new row to as follows and subsequently save it to a new dictionary named `new_dict` so that we don't mess up the original dictionary.

```r
dict %>% 
  bind_rows(tibble(
    model_varname = "IndProd", # this is free to choose but must be unique
    full_name = "An index of Industrial Production",
    database  = "eurostat",
    variable_code = "PRD", # in this case use the bt_indicator information here
    dataset_id = "sts_inpr_q",
    var_col = "indic_bt", # here we specify what the column with the variables is called
    freq = "q", # for quarterly data, 'm' would be monthly
    geo = "AT",
    unit = "I15", # for index of 2015 = 100
    s_adj = "NSA", # not seasonally adjusted
    nace_r2 = "B-D")) -> new_dict
```

Now we can use this newly created dictionary in the model;

```r
specification <- dplyr::tibble(
  type = c(
    "n"
  ),
  dependent = c(
    "EmiCO2IndustryProcess"
  ),
  independent = c(
    "HICP_Gas + HICP_Electricity + IndProdIdx"
  )
)

model <- run_model(specification = specification,
                   dictionary = new_dict,
                   present = FALSE,
                   quiet = TRUE,
                   constrain.to.minimum.sample = FALSE)
```

