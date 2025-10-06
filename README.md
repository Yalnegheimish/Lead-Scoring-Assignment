# Real Estate Lead Conversion Scoring

This project develops a lead scoring model for a real estate company to help them prioritize leads and improve conversion rates. By analyzing a lead's online behavior, the model assigns a probability score indicating how likely they are to become a tenant. This allows the sales team to focus their efforts on the most promising prospects, increasing efficiency and deal closures.

---

## Problem Statement
A leading real estate company generates a high volume of leads from various online channels. Their sales team follows up with every lead, but this process is inefficient and results in a suboptimal conversion rate. The goal is to build a scoring system that ranks leads based on their likelihood to convert, enabling the sales team to prioritize their follow-ups effectively.

---

## Datasets
The project uses two primary datasets:
* `leads_data.csv`: Contains information about leads and their online behavior, such as website interactions and events triggered.
* `target_data.csv`: A boolean dataset indicating whether a lead successfully converted into a tenant or not.

---

## Methodology
The solution follows a structured machine learning workflow, from data exploration and cleaning to model deployment and evaluation.

### 1. Exploratory Data Analysis (EDA)
Initial investigation began with forming and testing three key hypotheses about the data:
1.  **Clients are assigned the same lead\_id each time they become a lead.** 
    * **Result:** False. Analysis showed that about 24.6% of clients were assigned different lead IDs over time.
2.  **Clients have a unique lead\_id (i.e., IDs are not reused).**
    * **Result:** False. A small number of lead IDs (3) were found to be associated with multiple clients.
3.  **Clients can have multiple interactions with the website.**
    * **Result:** True. Clients can have many interactions, with some engaging over 1,900 times, indicating varying levels of interest.

### 2. Data Cleaning & Feature Engineering
The data was prepared for modeling through several steps:
* **Cleaning**: Removed duplicate records from both datasets, dropped leads with conflicting outcomes (i.e., listed as both converted and not converted), and handled missing values by filtering for rows with a valid `ga_lead_id`.
* **Feature Selection**: Identified the most useful columns, discarding those with excessive missing data or low variance. Key features retained were `client_id`, `unique_events`, and `event_action`.
* **Feature Engineering**: Created new features to better capture lead behavior, including:
    * `visit_freq`: The total number of interactions a client has had.
    * One-Hot Encoding: Converted the categorical `event_action` column into numerical format.
    * Aggregated Features: Calculated the total count of unique events per lead.

### 3. Model Selection
Several classification models were evaluated to find the best performer for this task:
* Logistic Regression (Baseline)
* **Random Forest (Chosen Model)**
* XGBoost
* Support Vector Machine (SVM)
* Naive Bayes

The **Random Forest** model was selected because it demonstrated the best overall performance, particularly in its F1-score (0.256) and AUC-ROC (0.562), indicating a strong ability to distinguish between converting and non-converting leads.

### 4. Model Training & Testing
The final Random Forest model was optimized using `GridSearchCV` to find the best hyperparameters. The optimal parameters were identified as: `{'max_depth': None, 'min_samples_leaf': 1, 'min_samples_split': 10, 'n_estimators': 150}`. After training on the prepared data, the model's performance was validated on a hold-out test set, yielding the following results:

| Metric | Score |
| :--- | :--- |
| **Test Accuracy** | 70.6% |
| **Test Precision**| 31.0% |
| **Test Recall** | 40.5% |
| **Test F1-score**| 35.1% |
| **Test AUC-ROC** | 59.2% |


---

## Results & Benchmarking
The model's effectiveness was benchmarked against the company's current process (randomly contacting leads).

* **Success Rate without Model**: The baseline probability of a random lead converting to a tenant is **19.6%**.
* **Success Rate with Model**: By focusing on the top 100 leads scored by the Random Forest model, the predicted conversion rate rises to **65.4%**.

This represents a **45.8% improvement** over the existing process, providing a clear and efficient path to prioritizing high-potential leads.

---

## Recommendations
Based on the model's insights, the following actions are recommended for the client:

* **Prioritize High-Probability Leads**: Focus sales efforts on leads with the highest conversion scores to maximize the conversion rate.
* **Implement Timely Follow-ups**: Engage with top-scoring leads quickly and with personalized communication to increase the likelihood of closing a deal.
* **Continuously Monitor and Refine**: The model should be regularly updated with new data to adapt to evolving market trends and maintain its predictive accuracy.
* **Utilize Model Insights**: Analyze which features are most predictive of conversion to better understand customer intent and tailor marketing strategies.
* **Enhance Data Collection**: Consider adding optional fields to the lead submission form to gather more data that could potentially provide more precise predictions.

---

### Requirements
The project uses the following Python libraries:
* `pandas`
* `numpy`
* `scikit-learn`
* `xgboost`
* `imbalanced-learn`
* `jupyter`
