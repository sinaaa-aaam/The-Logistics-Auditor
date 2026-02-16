# Project Brief: The "Last Mile" Logistics Auditor

**Client:** Veridi Logistics (Global E-Commerce Aggregator)  
**Deliverable:** Public Dashboard, Code Notebook & Insight Presentation

---

## 1. Business Context
**Veridi Logistics** manages shipping for thousands of online sellers. Recently, the CEO has noticed a spike in negative customer reviews. She has a "gut feeling" that the problem isn't just that packages are late, but that the estimated delivery dates provided to customers are wildly inaccurate (i.e., we are over-promising and under-delivering).

She needs you to audit the delivery data to find the root cause. She specifically wants to know: **"Are we failing specific regions, or is this a nationwide problem?"**

Your job is to build a "Delivery Performance" audit tool that connects the dots between **Logistics Data** (when a package arrived) and **Customer Sentiment** (how they rated the experience).

## 2. The Data
You will use the **Olist E-Commerce Dataset**, a real commercial dataset from a Brazilian marketplace. This is a relational database dump, meaning the data is split across multiple CSV files.

* **Source:** [Kaggle - Olist Brazilian E-Commerce Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
* **Key Files to Use:**
    * `olist_orders_dataset.csv` (The central table)
    * `olist_order_reviews_dataset.csv` (Sentiment)
    * `olist_customers_dataset.csv` (Location)
    * `olist_products_dataset.csv` (Categories)

## 3. Tooling Requirements
You have the flexibility to choose your development environment:

* **Option A (Recommended):** Use a cloud-hosted notebook like **Google Colab**, or **Deepnote**, etc.
* **Option B:** Use a local **Jupyter Notebook** or **VS Code**.
    * *Condition:* If you choose this, you must ensure your code is reproducible. Do not reference local file paths (e.g., `C:/Downloads/...`). Assume the dataset is in the same folder as your notebook.
* **Dashboarding:** The final output must be a **publicly accessible link** (e.g., Tableau Public, Google Looker Studio, Streamlit Cloud, or PowerBI Web, etc.).

---

## 4. User Stories & Acceptance Criteria

### Story 1: The Schema Builder
**As a** Data Engineer,  
**I want** to join the Orders, Reviews, and Customers tables into a single master dataset,  
**So that** I can analyze a customer's location and their review score in the same row.

* **Acceptance Criteria:**
    * Load the raw CSVs into your notebook.
    * Perform the correct joins (e.g., join Reviews to Orders on `order_id`, join Customers to Orders on `customer_id`).
    * **Check:** Ensure you don't accidentally duplicate rows (a common error with 1-to-many joins).

### Story 2: The "Real" Delay Calculator
**As a** Logistics Manager,  
**I want** to know the difference between the "Estimated Delivery Date" and the "Actual Delivery Date,"  
**So that** I can see how often we are lying to customers.

* **Acceptance Criteria:**
    * Create a new calculated column: `Days_Difference` = `order_estimated_delivery_date` - `order_delivered_customer_date`.
    * Classify orders into statuses: "On Time", "Late", and "Super Late" (> 5 days late).
    * Handle missing values: Some orders were never delivered (`order_status` = 'canceled' or 'unavailable'). These should be excluded or flagged separately.

### Story 3: The Geographic Heatmap
**As a** Regional Director,  
**I want** to see which specific States (`customer_state`) have the highest percentage of late deliveries,  
**So that** I can focus my repair efforts on the worst regions.

* **Acceptance Criteria:**
    * Calculate the % of late orders per State.
    * Visualize this on a map or a bar chart.
    * **Insight:** Identify if "Remote" states (far from the distribution center) are disproportionately affected.

### Story 4: The Sentiment Correlation
**As a** Customer Success Lead,  
**I want** to see if late deliveries actually cause bad reviews,  
**So that** I can prove to the CEO that logistics is the problem.

* **Acceptance Criteria:**
    * Create a visualization comparing "Delivery Delay (Days)" vs "Average Review Score (1-5)".
    * Show the average review score for "On Time" orders vs. "Late" orders.

---

## 5. Bonus User Story: The "Translation" Challenge
**As a** Global Analyst,  
**I want** to see product categories in **English**, not Portuguese,  
**So that** I can understand if "Furniture" is harder to ship than "Electronics".

* **Acceptance Criteria:**
    * The `product_category_name` is in Portuguese (e.g., `cama_mesa_banho`).
    * Use the `product_category_name_translation.csv` file included in the dataset (or create your own mapping) to translate these into English for your final dashboard.

---

## 6. The "Candidate's Choice" Challenge
**As a** Creative Problem Solver,  
**I want** to include one extra feature or analysis that adds specific business value,  
**So that** I can demonstrate my ability to think beyond the basic requirements.

* **Instructions:**
    * Add one more metric, chart, or drill-down.
    * **Requirement:** You must justify *why* this feature matters to the business in your README.

---

## 7. Submission Guidelines
Please edit this `README.md` file in your forked repository to include the following three sections at the top:

# Machine-Learning-Challenge
# The Logistics Auditor

## A. Executive Summary

Veridi Logistics (Olist) experiences late deliveries in approximately 8–12% of orders overall, with Northeastern and Northern states showing significantly higher rates (e.g., AL 20.8%, MA 17.1%, SE 15.0%) compared to SP (~3–5%). Late and especially Super Late deliveries strongly correlate with poor customer reviews (average scores of 3.0 and 1.7 respectively, versus 4.3 for On Time), confirming that inaccurate estimated delivery dates are a major driver of negative sentiment and potential customer churn. A prototype LLM-powered recommendation engine was developed to suggest faster, more reliable alternative product categories for customers in high-risk regions or categories (e.g., avoiding furniture/bulky items in Northeastern states), offering a practical way to maintain trust and reduce disappointment even when delays occur. High-late categories (e.g., furniture and bulky items) exhibit roughly 2× higher delay rates, highlighting clear opportunities for targeted carrier adjustments and ETA refinements.

## B. Project Links

- **Notebook**: [Google Colab Notebook 1](https://colab.research.google.com/drive/17L69MINJDPQG95oHFxiqyJM2BTvxvIg4?usp=sharing)  ,
- [Google Colab Notebook 2](https://colab.research.google.com/drive/1hTi7FM645k_CP8bxtTbkrC6yMAcB8rOb#scrollTo=3S2OBCZ9U6eN)
  

- **Dashboard**: [Dashboard](https://1ba64bf49a4fb06bc8.gradio.live/)

- **Presentation**: [Presentation Slides](https://www.canva.com/design/DAHBe0Jys5k/mDTndpBLV6Op-pV5X5RsDA/edit?utm_content=DAHBe0Jys5k&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton)  


## C. Technical Explanation

- **Data Cleaning**  
  Dates were converted to datetime with `errors='coerce'` to safely handle invalid values. Missing `review_score` values were imputed using the median (3.0). Non-delivered orders (statuses: canceled, unavailable, invoiced, processing, shipped without delivery date) were flagged as 'Not Delivered' and excluded from percentage-late calculations. Joins were performed carefully (`how='left'` or `how='inner'`) to prevent unintended row duplication.

- **Candidate's Choice Addition**  
 Built a prototype LLM-powered product recommendation engine (using free-tier API).
The system takes customer state, purchased category, and delivery risk as input, then suggests 3 alternative categories that are more reliable/faster in that region and have higher historical satisfaction.
Business value: In high-late-risk areas or categories, proactive alternative suggestions can maintain customer trust, reduce disappointment from delays, increase conversion on related items, and indirectly lower churn even when logistics issues occur. This turns a pain point into an upsell/cross-sell opportunity.

## Tech Stack

- **Data Processing & Analysis**: Python 3.12, Pandas (for joins, aggregations, cleaning), Plotly Express (for interactive charts)
- **Machine Learning / LLM**: Groq API + OpenAI client library (for Llama 3.1 model inference)
- **Dashboard**: Gradio (for interactive web app with filters, plots, and LLM recommendations)
- **Environment**: Google Colab (for development and sharing)
- **Dataset**: Olist Brazilian E-Commerce Public Dataset (from Kaggle) – raw CSVs processed into master dataset

## Instructions / Files Used

This project uses the Olist dataset split across multiple CSVs. All raw data is processed in the notebook to create a unified "master" dataset for analysis and dashboarding.

### Key Files:
- **Raw CSVs** (not uploaded to repo – too large; download from Kaggle: [Olist Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)):
  - `olist_orders_dataset.csv`: Core orders with timestamps, status, estimated/actual delivery dates.
  - `olist_order_reviews_dataset.csv`: Review scores and creation dates.
  - `olist_customers_dataset.csv`: Customer locations (state, city).
  - `olist_order_items_dataset.csv` & `olist_products_dataset.csv`: Product details and categories.
  - `product_category_name_translation.csv`: Portuguese-to-English category mapping (for bonus story).

- **Processed CSV** (uploaded to repo):
  - `master_logistics_audit.csv`: Unified dataset (~99k rows) after joins, cleaning, and feature engineering (e.g., `Days_Difference`, `Delivery_Status`, translated categories). Used as input for dashboard and analysis.

- **Notebook Files** (uploaded to repo):
  - `data_analysis_notebook.ipynb`: Main Jupyter notebook with data loading, cleaning, joins, delay calculations, geographic/sentiment analysis, and LLM prototype code. Includes all charts and outputs.
  - `app.py`: Gradio app script for the interactive dashboard (run with `gradio app.py` or in Colab). Features state filters, plots, and live LLM recommendations.

### How to Run Locally / Reproduce:
1. Download raw CSVs from Kaggle and place in a folder (e.g., `/data/`).
2. Open `data_analysis_notebook.ipynb` in Jupyter/Colab → run all cells to generate `master_logistics_audit.csv`.
3. Run the dashboard: `pip install gradio openai plotly pandas` → `python app.py` (or in Colab: paste and run the Gradio cell).
4. For LLM: Add your free Groq API key (console.g

**Important Note on Code Submission:**
* Upload your `.ipynb` notebook file to the repo.
* **Crucial:** Also upload an **HTML or PDF export** of your notebook so we can see your charts even if GitHub fails to render the notebook code.
* Once you are ready, please fill out the [Official Submission Form Here](https://forms.office.com/e/heitZ9PP7y) with your links

---

## 🛑 CRITICAL: Pre-Submission Checklist

**Before you submit your form, you MUST complete this checklist.**

> ⚠️ **WARNING:** If you miss any of these items, your submission will be flagged as "Incomplete" and you will **NOT** be invited to an interview. 
>
> **We do not accept "permission error" excuses. Test your links in Incognito Mode.**

### 1. Repository & Code Checks
- [ ] **My GitHub Repo is Public.** (Open the link in a Private/Incognito window to verify).
- [ ] **I have uploaded the `.ipynb` notebook file.**
- [ ] **I have ALSO uploaded an HTML or PDF export** of the notebook.
- [ ] **I have NOT uploaded the massive raw dataset.** (Use `.gitignore` or just don't commit the CSV).
- [ ] **My code uses Relative Paths.** 

### 2. Deliverable Checks
- [ ] **My Dashboard link is publicly accessible.** (No login required).
- [ ] **My Presentation link is publicly accessible.** (Permissions set to "Anyone with the link can view").
- [ ] **I have updated this `README.md` file** with my Executive Summary and technical notes.

### 3. Completeness
- [ ] I have completed **User Stories 1-4**.
- [ ] I have completed the **"Candidate's Choice"** challenge and explained it in the README.

**✅ Only when you have checked every box above, proceed to the submission form.**

---
