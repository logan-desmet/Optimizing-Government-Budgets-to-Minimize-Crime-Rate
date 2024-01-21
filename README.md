# Optimizing Government Budgets to Minimize Crime Rate

This [model](https://colab.research.google.com/drive/1TgD1DPvcEBcBBCTr8JKGETSXEAuC_xS5?usp=sharing) optimizes state budget decisions to minimize crime rates - so that future budgetary decisions can be made with data driven insights.

I encourage users to access the presentation above, which does a much better job at explaining the project than what is possible with the format below:

Budget Data from *Census.gov* is broken officially into these categories (within the budget data files):
![image](https://github.com/logan-desmet/Optimizing-Government-Budgets-to-Minimize-Crime-Rate/assets/150872110/71aa669b-b49b-499f-a8de-216a064e4a0b)

This finished model (assessible [here](https://colab.research.google.com/drive/1TgD1DPvcEBcBBCTr8JKGETSXEAuC_xS5?usp=sharing)) can take any US state and optimize its budget to minimize expected crime rates. It ouputs the optimized budget categories, given the state's total budget, as well as its "Interest on General Debt" and "General Expenditure, n.e.c." - which are both predetermined. Furthermore, the model constrains to the historic range of budget categories in the training data, ranging from 2015 to 2021 (excluding 2020).

Data Sourcing:
- Crime data: [FBI: Crime Data Explorer](https://cde.ucr.cjis.gov/LATEST/webapp/#/pages/home)
- Population Profile Data: [ACS Population Profiles](https://data.census.gov/table/ACSSPP1Y2015.S0201?t=Educational%20Attainment:Employment:Health%20Insurance:Income%20and%20Poverty:Renter%20Costs&g=010XX00US$0400000&y=2015&moe=false)
- Budget Data: [Census.gov - 2020 Budget Data, for example](https://www.census.gov/data/datasets/2020/econ/local/public-use-datasets.html)

Construction of the Model:
Crime is influenced by population profiles and statistics, which are inturn influenced by budgetary decisions (it is not possible/reasonable to say that budgets directly influence crime). So the model simply put follows the logic that budget data influences population profile data, and then population profile data influences crime data.

The first step was to combine all the historic datasets into one analysis-ready dataset. This was a long process and is fully documented in the data construction folder above, which includes read-me files on how to construct and combine the data with the provided Tableau Prep and Python files. The population profile survey was not administered in 2020 (due to COVID-19), and the FBI crime data converted its system for counting crimes in 2015 - so the range of possible data for the model was 2015-2021 (excluding 2020). Furthermore, crime data was made up of two crime types: violent crimes and property crimes. Inorder to stop the data from being skewed by states with larger budgets and residents all data was converted. Budget data was converted to proportions of total budget, crime data was converted to crimes per resident, and population profile data was converted to proportions of population.

The next step was to construct all the math behind the optimization model. First, a neural network was used to calculate feature importance among the population profile data points to predict crime rates, and using feature importance the top 10 most important features were selected. These selected population profile features were then used in a linear regression model to predict violent crime rate and property crime rate separetely. The budget category features were then used in a linear regression model to predict each of the selected population profile featuyres separately. Finally, the weights (coefficients and intercepts) from each model were then extracted and put into functions to use for the optimization model (with scaling when necessary).

Lastly, utilizing pyomo the functions and constraints were fit to the model, and solved using the 'cbc' solver. Additionally, functions were constructed to take the raw data and convert it into a format accessible/appropriate for the model. The user can simply upload their budget data in place of the 2021 budget data file, which is currently used as an example, and the code will neatly ouput the expected crime rate and optimized budget categories.

Key assumptions:

...

Model Performance:

...


