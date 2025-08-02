
### Overview

This notebook provides a comprehensive walkthrough of a data analysis and machine learning project focused on customer segmentation. It demonstrates how to leverage Google Cloud's powerful tools—BigQuery for large-scale data processing, BigQuery ML (via BigFrames) for clustering, and the Gemini Pro model for creative content generation—to derive actionable business insights from raw e-commerce data.

The primary goal is to segment customers based on their purchasing behavior (Recency, Frequency, Monetary Value) and then use these segments to create targeted marketing campaigns.

### Technologies & Libraries Used

*   **Google Cloud:**
    *   Vertex AI
    *   BigQuery
    *   Colab Enterprise
*   **Python Libraries:**
    *   `google-cloud-bigquery`
    *   `google-cloud-aiplatform`
    *   `bigframes` (for interacting with BigQuery data using a pandas-like API)
    *   `pandas`
    *   `matplotlib` (for data visualization)
    *   `vertexai` (for using the Gemini model)

### Workflow

1.  **Data Preparation in BigQuery:**
    *   A SQL query is executed to process the `bigquery-public-data.thelook_ecommerce.order_items` public dataset.
    *   A new table, `ecommerce.customer_stats`, is created. This table aggregates customer data to calculate RFM (Recency, Frequency, Monetary) metrics: `days_since_last_order`, `count_orders`, and `average_spend`.

2.  **Data Exploration with BigFrames:**
    *   The `customer_stats` table from BigQuery is loaded into a BigQuery DataFrame (`bqdf`).
    *   Standard exploratory data analysis is performed using familiar pandas methods like `.head()`, `.info()`, and `.describe()` to understand the data's structure and statistics.

3.  **Customer Segmentation with K-Means:**
    *   The data is split into training and test sets.
    *   A K-Means clustering model with 5 clusters is created and trained on the RFM features using `bigframes.ml.cluster.KMeans`.
    *   The trained model is saved directly back into BigQuery for persistence and future use.
    *   Predictions are made on the test data to assign each customer to a specific cluster.

4.  **Visualization and Analysis:**
    *   The clusters are visualized using a `matplotlib` scatter plot to show the relationship between `days_since_last_order` and `average_spend`, colored by cluster.
    *   The centroids of the K-Means model are queried from BigQuery using `ML.CENTROIDS` to analyze the defining characteristics of each customer segment.

5.  **Generative AI for Marketing Strategy:**
    *   The Vertex AI SDK is initialized to use the Gemini Pro model.
    *   A detailed prompt is engineered, providing the model with the summarized data for each cluster.
    *   The model is asked to act as a creative brand strategist and generate a unique persona, a catchy campaign title, and a step-by-step marketing action plan for each of the 5 customer segments.
