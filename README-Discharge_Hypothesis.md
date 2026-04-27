# The Cost of Change: Tracking Treatment Duration Through a 2025 Facility Reorganization

## 1. Research Question

_What are you investigating, and why does it matter?_

Wayside Recovery is a grant funded substance use treatment service provider located in the Twin Cities. In August 2025 it went through a re-organization due to budget cuts. Leadership is interested in how this organizational change has impacted client experience. 

The overall question is: Did the re-organization have an impact on the length of time clients stay in the program?

More specifically: Is there a difference between the median and average length of stay for successful and non-successful discharges in the six months before versus six months after the re-organization?

## 2. Hypothesis

_State your null and alternative hypotheses clearly and succinctly._

**Null hypothesis:**
    - There is no change in the length of stay for successful vs non-successful discharges between the six month period before and after the re-organization
  
**Alternative hypothesis:**
    - There is a change in the length of stay for successful vs non-successful discharges between the period before and after the re-organization. 

## 3. Data Description

_Describe your data source(s):_

_* Where it comes from (URL, API, dataset name)_ 
_* What each observation represents (unit of analysis)_ 
_* Number of observations and key variables_ 
_* Any filtering, cleaning, or transformation steps_

The data is from an electronic health record (EHR) system where discharge summaries are created and stored for each individual that leaves the program. Any information that could be used to identify individual clients has been stripped. 

Each observation represents a single treatment episode. There are clients that return to treatment multiple times due to the nature of substance use disorder. This analysis will take reoccurring clients into account by treating the subject ID as the grouping variable for the permutation test.

The total number of observations (n=332) include clients that have discharged from the program between March 2025 and February 2026. This allows for an equal 6 month comparison window surrounding the reorganization that occurred at the end of August 2025. The first period will include clients that discharged between March 1st, 2025 and August 31st, 2025. The second period includes clients that discharged between September 1st, 2025 and February 28th, 2026.

**Key variables:**
 - Subject ID (original identifiers have been replaced with synthetic subject IDs to protect client privacy)
 - Reason for discharge:
   - Completed Program
   - Patient left without staff approval
   - Patient conduct (behavioral)
   - Transferred to another program
   - Other 
     - The frequency of the "Transfer" and "Other" categories is low, so only "completed", "left without staff approval", and "behavioral" discharges will be included in the analysis.
 - Discharge Type (assigned based on 'Reason for Discharge')
   - Successful: includes clients marked as "completed Program"
   - Unsuccessful: includes clients in the "patient left without staff approval" and "patient conduct (behavioral)"
 - Intake date 
 - Discharge date 
 - Period (assigned based on discharge date)
   - Before = 6 month period before reorg
   - After = 6 month period after reorg
 - Length of stay (calculated column)
   - This is calculated as discharge date minus intake date PLUS one. This is done in the residential treatment setting as the client receives services the day they arrive on site. 


## 4. Methods

_Summarize how you analyzed the data:_

_* The test statistic for your permutation test_
_* How you simulated or resampled under the null hypothesis_
_* The metric(s) for which you created bootstrap confidence intervals_
_* Why the CLT does not apply to at least one metric_

The analysis will start by dividing the episodes by each discharge type (successful vs non-successful). The test statistic for the permutation test is the difference in the average and median length of stay in the period before the reorganization minus the average length of stay after the reorganization within each discharge category.

To sample under the null hypothesis, the period labels will be shuffled by switching up the Subject IDs. This grouped approach ensures that the non-independence for clients who return to treatment is preserved throughout the analysis. 

For the bootstrap confidence intervals, the metric is the difference in median length of stay between the 6 months before and after the reorganization. CLT does not apply to the median because it is not a proportion. 

## 5. Results

_Present your main findings:_

_* Key summary statistics and visualizations_
_* Observed test statistic and p-value (if applicable)_
_* Bootstrap confidence intervals for relevant metrics_

**Key Statistics:**
 - Mean & Median length of stay before the re-organization for each discharge type
 - Mean & Median length of stay after the re-organization for each discharge type
 - The observed difference between before and after for mean and median
 - P-value comparing the null hypothesis to the alternative for each discharge type
 - Confidence intervals for mean and median for each discharge type
 - Standard Deviation & IQR 

**Key Visualizations:**
 - Histogram of the length of stay by discharge type
 - Box plots for each discharge type to compare before and after distributions side by side
 - Permutation distribution (2) to represent the null hypothesis and p-value
 - Bootstrap distribution (2) to show uncertainty for median
  
## 6. Uncertainty Estimation

_Discuss your resampling results:_

_* How many resamples you used_
_* What the bootstrap or randomization distributions looked like_
_* How you interpret the interval estimates_

I plan to resample 1,000 times.

## 7. Limitations

_Briefly note any limitations in data, assumptions, or methods, including sources of bias or missing data._

 - Mixed-exposure bias: Because this analysis focused on the clients that discharged in the 6 months before and after the reorganization, some of the clients included in the "after" phase were on site in the "before" phase.
 - Residual interdependence: Even with the grouped permutation test, treatment episodes are not truly independent and are influenced by many factors.
 - Right-Censoring: There is some right-censoring at the end of the after period because clients that admitted during the period but did not discharge until after the end date were not included. The mean and median length of stay for the "after" period are slightly underestimated due to this. 
 - Selection Bias: Because "transfer" and "other" discharge types were excluded; and "left without staff approval" and "patient conduct" were grouped together, this analysis is only generalizable to successful vs unsuccessful discharges. 
 - Confounding variables: the length of stay may have changed due to changes external to Wayside. For example, Operation Metro Surge occurred in the "after" phase which also could have plausibly affected the amount of time clients stayed on site. 

## 8. References

_List all datasets, tools, libraries, or papers you cited._

Data source:
 - Internal EHR data from Wayside Recovery

Software/Libraries:
 - Python
 - Pandas
 - Matplotlib/Seaborn
 - NumPy


---

**Reminder:** Your README should be clear enough that someone unfamiliar with your work could understand what you studied, how you analyzed it, and what you found.
