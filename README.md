# Time in Treatment: Tracking Treatment Duration Through a 2025 Facility Reorganization

## At a glance: 
In late 2025, Wayside Recovery navigated a reorganization driven by federal funding shifts and industry wide staffing challenges. This analysis evaluates whether this period of change impacted the **length of stay**, a primary indicator of treatment stability for women in residential care.

By applying grouped permutation testing to 243 treatment episodes, this analysis accounts for the non-independence of returning clients to provide a statistical comparison of the six months before and after the transition. The findings reveal resilience: despite significant organizational upheaval, client time in treatment remained statistically stable.

## Context & Project Motivation
### About Wayside Recovery
Wayside Recovery is a Minnesota-based provider of co-occurring substance use treatment specifically tailored for women. Its Women’s Treatment Center (WTC) operates as a no-refusal program, ensuring access to recovery regardless of a client's ability to pay.

 - **Services Provided:** Co-occurding recovery treatment model, mental health care, targeted case management, peer recovery support, and family reunification assistance.

 - **Funding Model:** A complex mix of government grants, private foundations, insurance, and individual donors.

 - **The Mission:** "Breaking the cycle of addiction & trauma for women and their children."

### The 2025 Reorganization
The landscape for government funding in Minnesota has become increasingly volatile, with shrinking federal grants and stagnant insurance reimbursement rates.

 - **August 2025:** Faced with rising operational costs, leadership initiated a survival-driven staffing reorganization.

 - **The Impact:** Staffing reductions led to a drop in retention, leaving the WTC short-staffed for approximately four months and limiting intake capacity.

### Current Status (April 2026)
Six months after the reorganization, the program has successfully adapted and evolved:

 - **Fully Staffed:** Return to standard operational capacity.

 - **Enhanced Services:** Added on-site Medicated Assisted Treatment (MAT), expanded mental health groups, and resumed family involvement in treatment services

### Why Length of Stay Matters
While standard evaluations for SUD treatment often focus on "beds filled" or "success rates," this analysis focuses on Length of Stay as the primary health metric.

 - **The Problem with Volume Metrics:** High occupancy tells us that a program is full, but it doesn't reveal the quality or stability of the treatment.

 -  **Value of Length of Stay:** Pinpointing exactly when clients leave allows leadership to implement targeted interventions. For example, if spikes in unsuccessful discharges occur at day 30, it indicates a need for increased support during that specific phase of care.

This project examines if the 2025 reorganization fundamentally shifted these treatment timelines

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

 - **First period (Before):** clients that discharged between March 1st, 2025 and August 31st, 2025.

 - **Second period (After):** clients that discharged between September 1st, 2025 and February 28th, 2026.

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
   - Calculated as discharge date minus intake date PLUS one. This is done in the residential treatment setting as the client receives services the day they arrive on site.


##  Methods

To evaluate the impact of the reorganization, the analysis utilizes a non-parametric permutation test strategy. This approach avoids the assumptions of normality required by standard t-tests. This was an essential choice given that length of stay data is skewed.

 1. **Comparison Groups:** 
   The analysis segments treatment episodes into two primary categories to ensure a like-for-like comparison:
    - Successful Discharges: Clients who completed the program.
    - Unsuccessful Discharges: A combined group including "Patient left without staff approval" and "Patient conduct."
 2. **Test Statistics:**
   For each discharge category, the following test statistics were calculated to measure the shift between the 6-month periods:
    - Difference in Means: 
      - $\Delta\overline{x} = \overline{x}_{\text{Before}} - \overline{x}_{\text{After}}$
    - Difference in Medians: 
      - $\Delta\tilde{x} = \tilde{x}_{\text{Before}} - \tilde{x}_{\text{After}}$
 3. **Grouped Permutation Approach:**
   To accurately sample under the Null Hypothesis (assuming that the reorganization had no effect), the analysis utilizes Grouped Permutation
      - *Technical Note on Independence:* Because some clients return to treatment multiple times, individual treatment episodes are not truly independent. To maintain statistical integrity, Subject ID is used as the grouping variable during the shuffle.
      - **Process:** During each permutation, all episodes belonging to a single Subject ID are kept together and assigned the same "Period" label (Before or After).
      - **Benefit:** This preserves the non-independence of recurring clients, preventing inflated Type I errors and ensuring the resulting p-values are robust and defensible.
 4. **Uncertainty Estimation**
   Because means and medians have different mathematical properties, two distinct methods were used to calculate uncertainty:
    | Metric | Uncertainty Measure |  Reasoning | 
    |:---------------|:--------------|:---------------|
    | Means | Standard Error (SE) of the Difference | The permutation distribution for the mean is nearly symmetric and bell-shaped, allowing for SE calculations. |
    | Medians | 95% Bootstrap Confidence Intervals | The Central Limit Theorem (CLT) does not apply to medians. Bootstrapping provides a more reliable estimation of uncertainty for this non-proportional metric. |


## Results

The results of the permutation analysis suggest that the re-organization has not yet produced a statistically significant change in the length of stay for either successful or unsuccessful discharges. Across all four tests, the data consistently failed to reject the null hypothesis at the $\alpha = 0.05$ level, largely due to high variability within the population.

### Mean Length of Stay (Permutations #1 & #2)
These tests evaluated the change in the average length of stay in the period before vs the period after the re-organization. While the "Successful" group showed stability, the "Unsuccessful" group exhibited a visible, though non-significant, trend.

![alt text](image.png) ![alt text](image-1.png)

| Group | Test Stat (Mean Difference) | Observed Value |  p-value | Uncertainty (SE of Difference) |
|:---------------|:--------------|:---------------|:---------------|:---------------|
| Successful | Mean(before) - Mean(after) | 0.31 days | 0.945 | ±4.08 |
| Unsuccessful | Mean(before) - Mean(after) | 4.11 days | 0.181 | ±3.05 |


### Median Length of Stay (Permutations #3 & #4)
Switching to medians revealed larger absolute differences (6 days for both groups), but the high noise and wide confidence intervals prevented these findings from reaching statistical significance.

![alt text](image-2.png) ![alt text](image-3.png)

| Group | Test Stat (Median Difference) | Observed Value |  p-value | Uncertainty (Bootstrapped CI) |
|:---------------|:--------------|:---------------|:---------------|:---------------|
| Successful | Median(*before*) - Median(*after*) | 6 days | 0.239 | -13.00, 6.02 days |
| Unsuccessful | Median(*before*) - Median(*after*) | 6 days | 0.181 | -2.00, 11.00 days |

### Overall Findings
 - **Stability in Success:** For successful discharges, the mean length of stay is essentially unchanged (0.31-day difference). The larger 6-day median difference suggests that while the "average" is stable, the "typical" patient experience may be shifting, though high variance currently masks this.

 - **Variability in Unsuccessful Discharges:** Both mean (4.11 days) and median (6 days) tests for unsuccessful discharges show larger shifts than the successful group. However, the wide confidence intervals in these tests indicate that this population has high inherent variability, making it difficult to attribute changes specifically to the re-organization.

 - **Conclusion:** The re-organization has not significantly altered length of stay. The observed differences—particularly the 4.11 to 6-day shifts—likely represent "noise" or a "non-significant trend" that may require a larger sample size or a longer observation period to validate.


## Uncertainty Estimation

 - **Resampling:** The analysis used 1,000 permutations for each test statistic. The null distribution was created by shuffling the "Period" labels (Before vs. After) across unique subject IDs. This approach maintains the exchangeability of the data while accounting for potential clustering within treatment episodes. It prevents inflated Type I Errors and ensures the resulting p-values are not deflated by clients who come back for additional treatment. 

 - **Distribution Shape:** The permutation distribution for the mean differences was nearly symmetric with a bell-shaped curve, while the distribution of the median permutation appeared bi-modal and segmented. The difference in the shapes of the mean and median permutation distributions are due to the Central Limit Theorem applying to permutations for means. 
  
 - **Interval Estimates:** 
   - *The Standard Error of the mean difference* was used for the first two permutations.
     - The width of the interval was smaller for the permutation for successful discharges than the permutation for unsuccessful discharges. This means that there was less variability in discharge timeline for successful discharges.
     - The observed value for successful discharges fell within the first standard deviation, but for unsuccessful discharges the observed value was above the upper bound of the interval. 
       - This suggests that a change in length of stay for unsuccessful discharges has occured due to the re-organization, but it is not yet significant. 
       - On the other hand, the length of stay for successful discharges has not changed very much.
   - *Bootstrapped confidence intervals (95%)* were utilized to determine uncertainty for the difference in medians between the groups.
     - The width of the confidence intervals were wide for both the successful and unsuccessful groups. This indicates a high level of noise present in the underlying population and suggests that the observed differences are due to this noise rather than the re-organization.
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
