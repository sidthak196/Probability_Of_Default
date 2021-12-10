# Probability_Of_Default

## Introduction

The sudden increases in loan defaults is considered one of the main causes behind the
economic crisis of 2007 - 2008. After the financial crisis, the Financial Accounting Standards
Board (FASB) realized that there were changes that needed to be made to better prepare for
future conditions and make timely adjustments to reserve levels. With that in mind, they
introduced Current Expected Credit Losses (CECL). CECL requires expected losses to be
estimated over the remaining life of the loan. “The new accounting standard allows a financial
institution to leverage its current internal credit risk systems as a framework for estimating
expected credit losses.” (Federal Reserve)
Beyond the requirements of planning for economic downturn, predicting the probability
of default (PD) can help banks assess the creditworthiness of mortgagors thereby better deciding
on which applications to accept and set better acceptance thresholds for mortgagors.
Furthermore, it can help banks create plans for mortgagors who are late for payments in a way
that prevents them from defaulting or paying greater than their ability to do so. It was thus
required, and beneficial, that financial institutions predict the probability of default.
With all of this in mind, we have created a model that predicts the probability of default
using logistic regression and the Markov Transitions. We selected four transition states:
prepayment, current, delinquent, and default. We defined the states for delinquent loans to be
greater than 90 days and default to be 270 days in line with the federal loan standard. In this
paper, we will discuss our process in creating the model, the results that were obtained, and
finally the takeaways from the model and possible improvements to be included in the future.

Mortgage Data Source: 
Fannie Mae Single-Family mortgage loan data https://datadynamics.fanniemae.com/data-dynamics/#/userProduct

The loans originated in the period 2006 - 2008 to account for regular economic conditions,
the progression from regular to strained conditions, and strained conditions. The data was
partitioned and downloaded for each quarter in the years. Each file was processed and reduced
using pandas in Python.
We sampled the loans from California as we wanted to limit the changes between state
lines from legal and economic differences that may exist. We chose the servicer to be Bank of
America (BoA) to help control for policy changes between companies and how BoA selects
mortgagees to provide loans to. Furthermore, choosing a particular loan servicer also allows the
model to be more representative of a real life scenario where a client or employer will request a
PD to be calculated.

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
  In the univariate analysis we look at the trends of the endogenous variables and
determine their distributions for loan data samples. The key loan drivers that were observed were
Debt-to-Income, Loan-to-Value (LTV), Loan Age, FICO scores, Current and Actual Unpaid
Actual Balances.

<img width="567" alt="image" src="https://user-images.githubusercontent.com/21989815/145609152-8bc84a9b-08ec-4bd1-b339-1d7ec68b8209.png">

The debt to income ratio was fairly standard in its distribution. The distribution had peaks
in the 10s of the percentages. Debt-to-income is recorded at loan origin and is limited by not
considering increases in income. Loan-to-Value had a peak at the 80% mark indicating the
tendency of banks to process loans under the 80%. LTV mark to bypass the need for private
mortgage insurance

<img width="560" alt="image" src="https://user-images.githubusercontent.com/21989815/145609206-e2b55c5f-986e-432c-9353-801812208d46.png">

Similarly, FICOs had twin peaks at about 700 and 800 mark indicating predominance of
prime and Alt-A loans in the underwriting. Loan Age indicator in the dataset has a skew to the
right because in the dataset each loan is recorded from origination to its complete loan cycle. So,
there is a higher distribution of loans with younger loan age.

<img width="543" alt="image" src="https://user-images.githubusercontent.com/21989815/145609260-29e67049-4c41-46aa-b28c-0cca412bab65.png">

The distribution of the Actual UPB and the current UPB were fairly standard, with loans
having higher UPB at origination, with median value being at an excess of $300,000 and the
current UPB having lower value.

### Bivariate Analysis
  The first part of the bivariate analysis focused on the boxplots for different loan values
with respect to their target states: ‘Good Standing’, ‘Prepaid’,’Delinquent’ and ‘Default’.
Through the use of the boxplots, we get a fair idea about the movements in loan drivers affecting
the loan statuses.
For instance, the Debt-to-Income is lower for loans that remained in Good standing and those
that were prepaid. However, it declined for loans that were delinquent or defaulted as seen in plot
below:
<img width="474" alt="image" src="https://user-images.githubusercontent.com/21989815/145609460-06344011-7cab-4709-ae5e-ec14139df855.png">


Similarly, for loans in Good Standing and those that were prepaid, the Loan to Value ratio was
lower as one expects. Conversely, the LTV for loans that were delinquent or defaulted the LTV
were higher on a median value basis.
<img width="518" alt="image" src="https://user-images.githubusercontent.com/21989815/145609521-679da89b-a77d-47ff-be80-417d46443ee5.png">

A more realistic appraisal of these loans would be made possible with more dynamic
indicators like Current Loan to Value ratio, which not only tracks the outstanding loan but also
the current house price index. Such an indicator would not only hint at the level of loan burden
but also indicate the willingness to repay since this will give a better sense of the equity the
borrower has in the property

<img width="601" alt="image" src="https://user-images.githubusercontent.com/21989815/145609636-a223ee0d-765f-4b80-82d9-905fabf478fb.png">

The FICOs at origination were higher for loans that remained in good standing or were
prepaid. Logically, the Delinquent and Defaulted loans had lower FICOs. Similarly, the loan age
had lower range for good standing, delinquent and defaulted loans while the Loan age had a
higher range for loans that were prepaid. Similarly, the range and the interquartile range for loans
which defaulted or delinquent were shorter. This followed from the fact that once the loan
defaulted, the loan was not tracked for further activity.

<img width="588" alt="image" src="https://user-images.githubusercontent.com/21989815/145609753-59e2a3ef-203b-43ce-ac9b-ea444fb521da.png">

The unpaid principal balances were lower for loans in good standing while it was higher
for loans that were prepaid, delinquent or defaulted. This trend followed for current and actual
Unpaid Principal Balances. Higher UPB indicates higher stress on the loan servicing of the loan,
which was higher for loans that turned delinquent or those that defaulted.


### MacroEconomic Variables
  For the macroeconomic part, we selected several variables we think that are related to our
model and analyzed their correlations to keep only uncorrelated ones to simplify the dataset,
which are personal income, unemployment rate and CPI. Since we want to include as much
information as we can get from the data, the monthly difference of them was calculated to
generate 3 more columns. We also created lagged variables with the 6 columns we have so the
whole data set has 12 variables included, the original data, monthly differences of them ,and
lagged variables. Then we use PCA on the dataset to reduce the dimensions to 4 principal
components, with which we run the logistic regression model.

### Empirical Analysis
  The empirical probabilities were also analyzed as a benchmark. This was achieved by subsetting
the data using the current status of the loan. The frequencies of transitions from each status in
each subset of the data was used to construct the empirical probabilities.

  <img width="362" alt="image" src="https://user-images.githubusercontent.com/21989815/145609883-281392cf-b5fa-4eb5-9dd2-c2462f8e52c2.png">

The probabilities of default with time is calculated by grouping the data by dates and then
using the frequentist approach to get the probability of default

<img width="431" alt="image" src="https://user-images.githubusercontent.com/21989815/145609947-d1ba8da8-49ff-483c-afaa-b7baac7c7f36.png">

Similarly, the probability of prepayment was also calculated

<img width="438" alt="image" src="https://user-images.githubusercontent.com/21989815/145610080-f48a6c37-b589-4e60-af73-9e2bb4f33b40.png">

## Multinomial Logistic Regression
### Model Fitting
  After selecting the appropriate data, we applied Multinomial Logistic Regression. We
first created a column named “target” as the response variable. That’s the loans’ states next
month. Then we pick loans with the current state “delinquent”, fit the model, get the summary,
and make predictions.

<img width="582" alt="image" src="https://user-images.githubusercontent.com/21989815/145610408-289154b0-6593-482c-af20-6e4449cff126.png">

As we can see the table above is part of the summary and the probability of default. We would
repeat the same method again for the loans with state current. So that we can use both of these to
construct our transition matrix later. The table below is again part of the summary from current
to delinquent. I also want to mention that you may notice the same factor may have different
p-values when predicting different states. That happens a lot. For example, Current Actual UPB
is not significant at all when predicting from delinquent to default, but for current to delinquent,
its p-value is below 0.05.

<img width="376" alt="image" src="https://user-images.githubusercontent.com/21989815/145610564-4bfb96c7-c042-41bb-a9f2-711dedfdf052.png">

We also count the number of loans in each status and calculate the historical monthly average
probability of default for comparison. After that, we finally got our graph. The dashed line is the
predicted Default Probability. The scatter points are the historical default probability

<img width="351" alt="image" src="https://user-images.githubusercontent.com/21989815/145610598-f8ae9014-ce6d-468c-890e-88556ca58090.png">

We can see that the probability of default reached its peak around 10% in 2010. And data fans
out towards later years. That is because loans went in prepayment or default. So we have less
data points representing the later years. For example, you may notice in 2019 and 2020 there are
several points at 0. That's simply because our samples that are still active don't have default
during that month.

### Results and Conclusion

According to the results of logistic regression, we construct a transition matrix. Firstly,
we fill the results of logistic regression into the first column of the matrix named as “Monthly
Transition Matrix” like so:

<img width="278" alt="image" src="https://user-images.githubusercontent.com/21989815/145610732-c63c2b7c-83ca-462c-ae8d-a4d9425ce9b3.png">

We assume that all the loan status of the first month is “Current” and the initial dollar
amount is $100. Next we multiply the “Monthly Transition Matrix” of last month and “Monthly
Probability of being in State” of the last month to get the “Monthly Probability being in State” of
this month as well as the “Dollar Amount” of this month.

<img width="501" alt="image" src="https://user-images.githubusercontent.com/21989815/145610820-a4ab6f85-8d89-4b74-8779-100db0679b2b.png">

After that, multiply the “Monthly Transition Matrix” of last month and the default
probability of this month to calculate the “Transition Probabilities: State i to Default”.

<img width="515" alt="image" src="https://user-images.githubusercontent.com/21989815/145610895-187e825e-fe1c-4801-80ed-a03e574aac43.png">

Finally, we can calculate the cumulative probability of default by multiplying the
“Monthly Probability of being in State” and “Transition Probabilities: State i to Default”.

<img width="496" alt="image" src="https://user-images.githubusercontent.com/21989815/145610934-f646bf8e-54e1-43ed-a39a-8338d6822b7e.png">

The final results can be seen in the excel sheet in the folder. 

## Next Steps and Improvements
There are many things that we could do to improve this model. Firstly, we could increase
the number of transition states from only one delinquency level to many more that can account
for different levels of risk. If this model did come to fruition, we would also need to include a
sliding scale for the variables as the different states may require different weights of variables to
define the next transition. One thing that could be changed is the number of days of the transition
states. We used federal loan data transition states. However, when it comes to loans, the dates
should be in line with the BoA standards.
Due to time constraints and processing limitations, we were unable to expand our model
to include different service providers, longer time periods, and different states. We chose the
period of 2006 - 2008 to account for a variety of economic conditions. However, our model
would have been better if we were able to include a longer period of time. With all that said, if
we were to include other factors, we would also need to change the model to account for them.
For instance, if we expand our model to include the entire country, we would need to use
localized “macroeconomic” data to more accurately predict the probability of default. By
combining granular data with the grand scope of the country’s macroeconomic variables as well
as individualized data information the model could be greatly improved. However, it would
require more processing power and advanced programming techniques.
We also could consider more variables to be included for the probability of default model.
With different weighted variables and a way for the model weights to change using machine
learning techniques, the model could greatly be improved. The current model could also be
combined with a loss given default model to calculate the expected loss as required by CECL




