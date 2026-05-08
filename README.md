# Time in Treatment: Tracking Treatment Duration Through a 2025 Facility Reorganization

## At a glance: 
In late 2025, Wayside Recovery navigated a high-stakes reorganization driven by federal funding shifts and industry-wide staffing challenges. This analysis evaluates whether this period of "chaos" impacted the Length of Stay (LOS)—a primary indicator of treatment stability—for women in residential care.

By applying grouped permutation testing to 243 treatment episodes, this project accounts for the non-independence of returning clients to provide a rigorous statistical comparison of the six months before and after the transition. The findings reveal programmatic resilience: despite significant organizational upheaval, client time-in-treatment remained statistically stable, successfully preserving the "dose" of care essential to Wayside's mission.

## The Why

Wayside Recovery is a Minnesota based co-occuring substance use treatment provider that has programming specifically tailored to women. The Residential Womens Treatment Center (WTC) is a *no-refusal* program, meaning that everyone has a chance at recovery regardless of their ability to pay. Wayside is in a special position to provide these critical services to those with or without insurance through government grants, private foundations, and individual donors. This funding also allows us to provide mental health services, targeted case management, peer recovery supports, and family reunification assistance. 

Because of the changes in the federal government and the scruity Minnesota's Department of Human Services (MN DHS) is under, a significant portion of our funding is at risk. In Janurary of 2026 our largest federal grant was nearly terminated. There is a link to a NPR article at the end of this README covering this. The decision to terminate the grant was reversed, but it was another reminder that the landscape of government grants is constantly changing. 

There has been slow and painful reductions in the amount of state and federal funding available over the past few years. Additionally, insurance reimbursement rates have been slow to increase with the rising cost of program expenses. In **August 2025** Wayside leadship needed to re-organize the staffing structure in order to survive. Staffing was reduced across the board, and significant changes, both internal and external, fueled a drop in staff retention. This left WTC short staffed for about four months and limited the number of clients we could intake.

  **Our Mission: "Breaking the cycle of addiction & trauma for women and their children."**

The program is evolving and adapting. It is now fully staffed. We have added on-site Medicated Assisted Treatment (MAT) options so clients no longer need to go off site to get these services. Additional mental health and mutual support groups have been introduced. Family activities are starting back up again and mothers can have CPS visitations on site. 

March 2026 marked 6 months since the re-organization occured. Clients stay in residential treatment for 40-60 days, so I had enough discharge data when embarking on this project to possibly see the effect of the re-organization. The file *Discharge_Hypothesis_6mo.ipynb* holds this analysis. I have also included an update with the most recent available discharge data in *Discharege_hypothesis_8mo.ipynb*

The length of stay is a key variable for this analysis. Typically when evaluating a program the focus is on the percent of beds filled or the porportion of successful discharges. While these metrics tell us what is happening, using length of stay pinpoints when a programmatic issue is occuring. For example, if unsuccessful discharges spike near the end of treatment, leadership can implement targeted interventions to better support clients during that phase of care.

## Research Question

**The overall question is: How did the re-organization impact the length of time clients stay in the program?**

More specifically: Is there a difference between the median and average length of stay for successful and non-successful discharges in the six months before versus six months after the re-organization?


## Hypothesis

**Null hypothesis:**
    - There is no change in the length of stay for successful vs non-successful discharges between the six month period before and after the re-organization
  
**Alternative hypothesis:**
    - There is a change in the length of stay for successful vs non-successful discharges between the period before and after the re-organization. 

## Data Description

The data is from an electronic health record (EHR) system where discharge summaries are created and stored for each individual that leaves the program. Any information that could be used to identify individual clients has been stripped. 

Each observation represents a single treatment episode. There are clients that return to treatment multiple times due to the nature of substance use disorder. This analysis will take reoccurring clients into account by treating the subject ID as the grouping variable for the permutation test.

The total number of observations (n=243) include clients that have discharged from the program between **March 2025** and **February 2026.** This allows for an equal 6 month comparison window surrounding the reorganization that occurred at the end of **August 2025**.

 - **First period:** clients that discharged between March 1st, 2025 and August 31st, 2025.

 - **Second period:** clients that discharged between September 1st, 2025 and February 28th, 2026.

**Key variables:**
 - Subject ID (original identifiers have been replaced with synthetic subject IDs to protect client privacy)
 - Reason for discharge:
   - Completed Program
   - Patient left without staff approval
   - Patient conduct (behavioral)
   - Transferred to another program
   - Other 
     - The frequency of the "Transfer" and "Other" categories is low, so only "completed", "left without staff approval", and "conduct" discharges will be included in the analysis.
 - Discharge Type 
   - *assigned based on 'Reason for Discharge'*
   - Successful: includes clients marked as "completed Program"
   - Unsuccessful: includes clients in the "patient left without staff approval" and "patient conduct"
 - Intake date 
 - Discharge date 
 - Period 
   - *assigned based on discharge date*
   - Before = 6 month period before reorg
   - After = 6 month period after reorg
 - Length of stay 
   - *calculated column*
   - This is calculated as discharge date minus intake date PLUS one. This is done in the residential treatment setting as the client receives services the day they arrive on site.


##  Methods

The analysis divided the episodes by each discharge type (successful vs non-successful). 

The test statistic for the permutation test is the difference in the average and median length of stay in the period before the reorganization minus the average length of stay after the reorganization within each discharge category.

To sample under the null hypothesis, the period labels are shuffled by switching up the **Subject IDs.** This grouped approach ensures that the non-independence for clients who return to treatment is preserved throughout the analysis. With grouping, if a client has multiple treatment episodes they stick together throughout permutation and assigned the same period when shuffled. 

Uncertainty is calculated with the **Standard Error of the Difference** formula for the permuations tests related to means. **Bootstrap confidence intervals (95%)** will be used to determine uncertainty for permutation tests related to medians. The metric is the difference in median length of stay between the 6 months before and after the reorganization. CLT does not apply to the median because it is not a proportion. 

## Results

The results of the permutation analysis suggest that the re-organization has not yet produced a statistically significant change in the Length of Stay (LOS) for either successful or unsuccessful discharges. Across all four tests, the data consistently failed to reject the null hypothesis at the $\alpha = 0.05$ level, largely due to high variability within the population.

### Mean Length of Stay (Permutations #1 & #2)
These tests evaluated the change in the average length of stay in the period before vs the period after the re-organization. While the "Successful" group showed stability, the "Unsuccessful" group exhibited a visible, though non-significant, trend.

![alt text](image.png) ![alt text](image-1.png)

| Group | Test Stat (Mean Difference) | Observed Value |  p-value | Uncertainty (SE of Difference) |
|:---------------|:--------------|:---------------|:---------------|:---------------|
| Successful | $\overline{X}_{B} - \overline{X}_{A}$ | 0.31 days | 0.945 | ±4.08 |
| Unsuccessful | $\overline{X}_{B} - \overline{X}_{A}$ | 4.11 days | 0.181 | ±3.05 |


### Median Length of Stay (Permutations #3 & #4)
Switching to medians revealed larger absolute differences (6 days for both groups), but the high noise and wide confidence intervals prevented these findings from reaching statistical significance.

![alt text](image-2.png) ![alt text](image-3.png)

| Group | Test Stat (Median Difference) | Observed Value |  p-value | Uncertainty (Bootstrapped CI) |
|:---------------|:--------------|:---------------|:---------------|:---------------|
| Successful | $\tilde{x}_{B} - \tilde{x}_{A}$ | 6 days | 0.239 | [-13.00, 6.02] days |
| Unsuccessful | $\tilde{x}_{B} - \tilde{x}_{A}$ | 6 days | 0.181 | [-2.00, 11.00] days |

### Overall Findings
 - **Stability in Success:** For successful discharges, the mean length of stay is essentially unchanged (0.31-day difference). The larger 6-day median difference suggests that while the "average" is stable, the "typical" patient experience may be shifting, though high variance currently masks this.

 - **Variability in Unsuccessful Discharges:** Both mean (4.11 days) and median (6 days) tests for unsuccessful discharges show larger shifts than the successful group. However, the wide confidence intervals in these tests indicate that this population has high inherent variability, making it difficult to attribute changes specifically to the re-organization.

 - **Conclusion:** The re-organization has not significantly altered length of stay. The observed differences—particularly the 4.11 to 6-day shifts—likely represent "noise" or a "non-significant trend" that may require a larger sample size or a longer observation period to validate.

### What This Means for Wayside
The past year (3/1/25-2/28/26) has been one of significant chaos: navigating funding threats, federal scrutiny, and a major internal restructuring. This goal of this analysis to see if the August 2025 reorganization fundamentally changed how long women stay in our care.

**The Bottom Line**:

Wayside is holding steady. Despite the reduction in staffing and the stress of the reorganization, there hasn't been a drastic or "statistically clear" shift in the length of time clients stay in the program. Whether a client completes the program or leaves early, the timeline has remained relatively consistent with where it was before the changes. In this case, a non-significant result was a positive indicator.

Success remains stable. For women who successfully complete the program, their length of stay is almost identical to before the restructuring. This suggests that even with a leaner team  the time spent healing on-site hasn't been diluted.

**The "Noise" in the data:** While there was a 4 to 6 day difference in some areas (specifically for those who leave early), we can’t yet say for sure if that was caused by the reorganization. In a program like WTC, client stays vary naturally from month to month. Right now, those 4–6 days are considered "noise" - meaning they could just be normal, unpredictable fluctuations rather than a direct result of staffing changes.

What comes next? Because there is only six months of "after" data in this analysis, continuous monitioring is still needed. As more women graduate from the now fully-staffed program, we will get a clearer picture of whether these small trends turn into real, lasting changes.

**In short:** The reorganization was a survival necessity, and so far, the data suggests that Wayside has managed to protect the time in treatment for the women we serve, maintaining our mission even through a period of intense transition.

## Uncertainty Estimation

 - **Resampling:** The analysis used 1,000 permutations for each test statistic. The null distribution was created by shuffling the "Period" labels (Before vs. After) across unique subject IDs. This approach maintains the exchangeability of the data while accounting for potential clustering within treatment episodes. It prevents inflated Type I Errors and ensures the resulting p-values are not deflated by clients who come back for additional treatment. 

 - **Distribution Shape:** The permutation distribution for the mean differences was nearly symmetric with a bell-shaped curve, while the distribution of the median permutation appeared bi-modal and segmented. The difference in the shapes of the mean and median permutation distributions are due to the Central Limit Theorem applying to permutations for means. 
  
 - **Interval Estimates:** 
   - The Standard Error of the mean difference was used for the first two permutations.
     - The width of the interval was smaller for the permutation for successful discharges than the permutation for unsuccessful discharges. This means that there was less variability in discharge timeline for successful discharges.
     - The observed value for successful discharges fell within the first standard deviation, but for unsuccessful discharges the observed value was above the upper bound of the interval. This suggests that a change in length of stay for unsuccessful discharges has occured due to the re-organization, but it is not yet significant. On the other hand, the length of stay for successful discharges has not changed very much.
   - Bootstrapped confidence intervals were utilized to determine uncertainty for the difference in medians between the groups.
     - The width of the confidence intervals were wide for both the successful and unsuccessful groups. This indicates a high level of noise present in the underlying population and  suggests that the observed differences are due to roise rather than the re-organization.
     - Additionally, both observed differences in median length of stay fell within the 95% CI. 


## Limitations

 - **Mixed-exposure bias:** Because this analysis focused on the clients that discharged in the 6 months before and after the reorganization, some of the clients included in the "after" phase were on site in the "before" phase.
 - **Power:** With this analysis only including 234 episodes, it might be underpowered to detect small changes. This is why the 6 day median shift was not significant.
 - **Residual interdependence:** Even with the grouped permutation test, treatment episodes are not truly independent and are influenced by many factors.
 - **Right-Censoring:** There is some right-censoring at the end of the after period because clients that admitted during the period but did not discharge until after the end date were not included. The mean and median length of stay for the "after" period are slightly underestimated due to this. 
 - **Selection Bias:** Because "transfer" and "other" discharge types were excluded; and "left without staff approval" and "patient conduct" were grouped together, this analysis is only generalizable to successful vs unsuccessful discharges. 
 - **Confounding variables:** the length of stay may have changed due to changes external to Wayside. For example, Operation Metro Surge occurred in the "after" phase which also could have plausibly affected the amount of time clients stayed on site. 

## References

Data source:
 - EHR data from Wayside Recovery

Software/Libraries:
 - Python
 - Pandas
 - Matplotlib/Seaborn
 - NumPy

https://www.npr.org/2026/01/14/nx-s1-5677104/trump-administration-letter-terminating-addiction-mental-health-grants
