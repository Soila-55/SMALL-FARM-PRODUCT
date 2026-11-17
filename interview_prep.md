# Interview Preparation Document

This document provides answers to standard technical questions and two STAR-method behavioral stories highlighting skills developed during the "Small Farm Product" Capstone project.

---

## Part 1: Technical Questions

### 1. Explain a LEFT JOIN
A `LEFT JOIN` retrieves all records from the left table and the matching records from the right table. If there is no match, the result will contain `NULL` values on the side of the right table.
*   **Example:** When analyzing farm suppliers, if we `LEFT JOIN` the `Farmers` table (Left) with the `Transactions` table (Right) on `Farmer_ID`, we get a list of all farmers registered in our system, including those who have not made any sales transaction yet (represented by `NULL` in the transaction fields).

### 2. How do you handle missing data?
Handling missing data requires understanding why the data is missing (MCAR, MAR, or MNAR). My standard workflow is:
1.  **Identification:** Check missingness using `.isnull().sum()` in Pandas.
2.  **Imputation vs. Deletion:**
    *   For categorical features (e.g., *Crop_Type*), I impute with the mode or a placeholder like "Unknown".
    *   For numerical features (e.g., *Soil_pH*), I impute using the median (if skewed) or mean (if normally distributed).
    *   **Machine Learning Imputation:** Use KNNImputer for complex features.
    *   **Deletion:** Drop records only if missing values represent less than 5% of the dataset and do not introduce systematic bias.

### 3. Mean vs. Median: When do you use which?
*   **Mean:** The average of all data points. It is best used for symmetrical, normally distributed continuous data without outliers (e.g., average optimal storage temperature of farm products).
*   **Median:** The middle value when sorted. It is robust to outliers and skewed distributions. For example, when analyzing "Small Farm Product" sales values, a few massive commercial orders can skew the mean upwards, making the median a much more realistic representation of typical smallholder farmer income.

### 4. What does a p-value mean?
A p-value (probability value) is the probability of obtaining test results at least as extreme as the observed results, assuming the null hypothesis ($H_0$) is true.
*   **Threshold:** If the p-value is below our significance level (typically $lpha = 0.05$), we reject the null hypothesis.
*   **Context:** If we test a new fertilizer and get a p-value of 0.02, it means there is only a 2% chance that the observed yield improvement happened by pure random chance, allowing us to conclude the fertilizer has a statistically significant effect.

### 5. How would you investigate a sudden drop in sales?
I would conduct a systematic root cause analysis structured as follows:
1.  **Rule out data anomalies:** Verify that the data pipeline, ETL processes, or payment gateways are functioning and not dropping transaction logs.
2.  **Temporal & Segment Breakdown:** Determine if the drop is sudden (single-day event) or gradual. Breakdown the metrics by product category, region, customer segment, and payment platform.
3.  **External Factors:** Investigate weather disruptions affecting deliveries, seasonal agricultural patterns, market competitor pricing shifts, or macro-economic changes.
4.  **Funnel Analysis:** Evaluate user behavior metrics (drop-offs during checkout, cart abandonment rate increases) to isolate product friction points.

---

## Part 2: Behavioral STAR Stories

### Story 1: Optimizing Market Access for "Small Farm Product" (Analytical Capability)
*   **Situation:** Smallholder farmers using our platform faced severe losses due to high perishability rates and inefficient delivery channels, leading to a 15% drop in product value before reaching local markets.
*   **Task:** I was tasked with identifying bottlenecks in the distribution chain and optimizing distribution routes to maximize product quality and delivery efficiency.
*   **Action:** Using Python (`pandas`, `scipy`) and SQL, I aggregated transportation logs, temperature fluctuation data, and sales velocities. I built an optimization model that clustered farm locations and calculated the shortest route to local cold rooms.
*   **Result:** Reduced transit times by 22%, which cut crop spoilage by 18% and directly boosted farmers' net take-home margins by 12% within three months.

### Story 2: Handling Incomplete Farm Data (Problem-Solving & Collaboration)
*   **Situation:** Our data collection apps used by field officers had poor network coverage, causing 30% of critical crop-health and soil parameter fields to be missing.
*   **Task:** I needed to build a predictive yield model despite having highly incomplete and messy datasets.
*   **Action:** I hosted a feedback session with field officers to find out why the fields were blank. I then implemented a robust data processing pipeline in Python, utilizing `IterativeImputer` to impute missing soil attributes based on nearby geographical region features, and helped design an offline-first mobile synchronization feature for the field team.
*   **Result:** Restored 95% usability to the dataset and achieved an R-squared of 0.84 on our yield prediction model, while improving data ingestion reliability from the field by 40%.
