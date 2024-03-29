# Optimizing Government Budgets to Minimize Crime Rate

This model (accessible [here](https://colab.research.google.com/drive/1TgD1DPvcEBcBBCTr8JKGETSXEAuC_xS5?usp=sharing)) optimizes state budget decisions to minimize crime rates - so that any future budgetary decisions can be made with data-driven insights.

NOTE: I encourage users to access the presentation above, which does a much better job of explaining the project than what is possible with the format below:

---

Budget Data from *Census.gov* is broken officially into these categories (within the budget data files):
![image](https://github.com/logan-desmet/Optimizing-Government-Budgets-to-Minimize-Crime-Rate/assets/150872110/95115d0b-99cc-4598-9940-50247d73a234)

This finished model (accessible [here](https://colab.research.google.com/drive/1TgD1DPvcEBcBBCTr8JKGETSXEAuC_xS5?usp=sharing)) can take any US state and optimize its budget to minimize expected crime rates. It outputs the optimized budget categories, given the state's total budget, as well as its "Interest on General Debt" and "General Expenditure, n.e.c." - which are both predetermined. Furthermore, the model constrains to the historic range of budget categories in the training data, ranging from 2015 to 2021 (excluding 2020).

---

**Data Sourcing:**
- Crime data: [FBI: Crime Data Explorer](https://cde.ucr.cjis.gov/LATEST/webapp/#/pages/home)
- Population Profile Data: [ACS Population Profiles](https://data.census.gov/table/ACSSPP1Y2015.S0201?t=Educational%20Attainment:Employment:Health%20Insurance:Income%20and%20Poverty:Renter%20Costs&g=010XX00US$0400000&y=2015&moe=false)
- Budget Data: [Census.gov - 2020 Budget Data, for example](https://www.census.gov/data/datasets/2020/econ/local/public-use-datasets.html)

---

**Construction of the Model:**

Crime is influenced by population profiles and statistics, which are in turn influenced by budgetary decisions (it is not possible/reasonable to say that budgets directly influence crime). So the model follows the logic that budget data influences population profile data, and then population profile data influences crime data.

The first step was to combine all the historic datasets into one analysis-ready dataset. This was a long process and is fully documented in the data construction folder above, which includes read-me files on how to construct and combine the data with the provided Tableau Prep and Python files. The population profile survey was not administered in 2020 (due to COVID-19), and the FBI crime data converted its system for counting crimes in 2015 - so the range of possible data for the model was 2015-2021 (excluding 2020). Furthermore, crime data was made up of two crime types: violent crimes and property crimes. In order to stop the data from being skewed by states with larger budgets and more residents, all data was converted. Budget data was converted to proportions of the total budget, crime data was converted to crimes per resident, and population profile data was converted to proportions of the population.

The next step was to construct all the math behind the optimization model. First, a neural network was used to calculate feature importance among the population profile data points to predict crime rates, and using feature importance the top 10 most important features were selected. These selected population profile features were then used in a linear regression model to predict the violent and property crime rates separately. The budget category features were then used in a linear regression model to predict each of the selected population profile features separately. Finally, the weights (coefficients and intercepts) from each model were then extracted and put into functions to use for the optimization model (with scaling when necessary).

Lastly, utilizing pyomo the functions and constraints were fit to the model, and solved using the 'cbc' solver. The decision variables had to be set to 0 at the start of the model and then optimized with the constraints mentioned above. Additionally, functions were constructed to convert the raw data into a format accessible/appropriate for the model. The user can simply upload their budget data in place of the 2021 budget data file, which is currently used as an example, and the code will neatly output the expected crime rate and optimized budget categories.

---

**Key assumptions:**

- State residents act in similar ways (crime) with similar population profile data - meaning the model does not account for the difference culturally among states
  - *See 'Model Performance' below for projected impact*
- Accepted a minimum crime rate of 0.01 - it would be unreasonable to predict a crime rate of 0.0
- The budget total input into the model does not include "Direct Expenditures" (which is not one of the categories listed above) - the reason being this funding is split between national and state funding, and does not change much from state to state
  - *So with a total budget of $1 million, and a direct expenditures of $200k --> the model input is $800k*

---

**Model Performance:**

- The output of the machine learning model crime rate predictions were extremely accurate
  - All crime rate predictions were within 0.001 of the expected true rate - suggesting that culture does not make too large of an impact

---

**Conclusions and Projected Benefits:**

- ‘Education’, ‘Public Safety’, and ‘Environment and Housing’ are the most influential factors on expected crime rates
  - Every state maxed out the amount they could allocate to these budgetary categories. This suggests that the government currently is spending too little on these categories and more funding should be put in place. Moreover, with fewer funding limitations the optimization model should be rerun to produce better results
- Of the 10 most important features identified from the Neural Network: ‘health insurance coverage’ and ‘Proportion of Population above 25 with no Highschool degree’ both align with these most influential factors - ultimately suggesting that these should be heavily considered when making budgetary decisions



