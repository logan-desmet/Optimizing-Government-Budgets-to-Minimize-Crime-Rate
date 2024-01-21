The raw, downloaded budget file from census.gov is titled 'XXslsstab1X.xslx', with the first XX refering to the year and the last X refering to a or b for years 2015 and 2016 - years 2017-2021 the a and b came together ('XXslsstab1.xslx')

The raw data went through the following steps (with 2019 for example):

"19slsstab1.xlsx"
->
(1)budgetData_prep.ipynb (for 2017-21) / AB_budgetData_prep.ipynb (for 2015 and 2016)
->
"2019budgetData_prePrep.csv"
->
(2) budgetDataFlow_cycles.tfl
-> 
"19budgetData_post1prep.csv"
->
(3) budgetpostPrepColSelection.tfl (for 2017-21) / budgetpostPrepColSelection(A_B).tfl (for 2015 and 2016)
->
"19budgetFinal.csv"
->
(4) budgetComboFlow.tfl 
-> 
combines data to "budgetCombinedFinal.csv"
->
(5) budgetCombinedPrep.ipynb
->
"budgetCombinedPostPrep_FINAL.csv"
->
TAKEN TO DATA COMBINATION STAGE (see folder "Combining Data")