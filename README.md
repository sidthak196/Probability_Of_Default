# Probability_Of_Default
A project to model the probability of default for residential mortgages from California originating in 2006 to 2008. 
Mortgage Data Source: 

Fannie Mae Single-Family mortgage loan data https://datadynamics.fanniemae.com/data-dynamics/#/userProduct

Macro economic data: https://fred.stlouisfed.org/

## Modeling Techniques
1. Exploratory Data Analysis
2. Principal Component Analysis
3. Markov Transition Model
4. Multinomial Logistic Regression

## Model Assumptions
1. Markov Assumptions
  ![PD_project drawio](https://user-images.githubusercontent.com/21989815/142073033-1dd496a7-bab7-40ae-ad51-a857418d7916.png)
2. Multinomial Logistic regression
   - Homoscedasticity 
   - No Multicollinearity 
   - Normality
 
## Data Assumptions
- Data Filter: Only loans with servicer as Bank of America
- Definition of Delinquent, Default and Prepaid
  ![image](https://user-images.githubusercontent.com/21989815/142073769-11b6b144-ef7f-48cb-8de8-4e8d36910920.png)
- Macroeconomic Variables
  - House Price Index
  - Personal Income
  - Real Gross Domestic Product
  - Consumer Price Index
  - Unemployement Index
- Data Columns used in the model
  ![image](https://user-images.githubusercontent.com/21989815/142074063-16fca82b-f27f-4d9f-aca6-9e857370496a.png)

## Exploratory Data Analysis
### Univariate Analysis
### Bivariate Analysis
### MacroEconomic Variables
### Empirical Analysis

  - ![image](https://user-images.githubusercontent.com/21989815/142074902-04e82a56-346d-4506-a22d-bae2ca0aa79a.png) <br />
  This is the transition matrix calculated empirically using the frequentist appraoch. The  probabilites have been normalized to reflect the model assumptions. 

  - ![image](https://user-images.githubusercontent.com/21989815/142075150-954ab498-c71f-4b87-9472-daf9a9c692b7.png) <br />
  This is the probability of default plotted with time. 

  - ![image](https://user-images.githubusercontent.com/21989815/142075293-227ddf7c-ca9c-48b7-ae47-94f1bb6f82ed.png) <br />
  This is the probability of laon being in the prepaid state plotted with time. 

# Multinomial Logistic Regression




