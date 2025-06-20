Online Retail Sales Forecasting
Overview
This Jupyter notebook analyzes the OnlineRetail.csv dataset to build a forecasting model for predicting the quantity of products sold in an e-commerce setting. The analysis is performed using PySpark for data preprocessing and scikit-learn for building a linear regression model. The goal is to assist the Sales & Operations Planning (S&OP) team in planning for end-of-year sales by forecasting demand, managing inventory, and ensuring timely deliveries.
Objectives

Data Splitting: Split the dataset into training and test sets based on the date "2011-09-25". The training set includes data up to and including this date, while the test set includes data after it. The training set is returned as a pandas DataFrame (pd_daily_train_data) with columns Country, StockCode, InvoiceDate, and Quantity.
Model Evaluation: Calculate the Mean Absolute Error (MAE) of the forecasting model for the Quantity sold using the test set.
Weekly Forecast: Predict the total units sold during week 39 of 2011, stored as an integer (quantity_sold_w39).

Features

Data Preprocessing: Handles missing values, removes negative quantities, converts date formats, and encodes categorical variables (Country and StockCode).
Data Analysis: Uses PySpark for efficient handling of large datasets and pandas for final data manipulation.
Modeling: Employs a Linear Regression model to predict product quantities based on encoded Country and StockCode.
Evaluation: Computes MAE to assess model performance.
Forecasting: Aggregates quantities for specific weeks to support inventory planning.

Prerequisites
Ensure you have the following installed:

Python 3.6 or higher
Jupyter Notebook
Libraries listed in requirements.txt
Apache Spark (for PySpark)

Installation

Clone or download the repository.
Install the required dependencies by running:pip install -r requirements.txt


Ensure the dataset OnlineRetail.csv is available in the specified directory or update the file path in the notebook.
Ensure Apache Spark is installed and configured for PySpark. Set up the Spark environment by configuring SPARK_HOME and adding it to your system path.

Usage

Open the Jupyter notebook:jupyter notebook final_hw_a39948.ipynb


Run the cells sequentially to:
Load and preprocess the OnlineRetail.csv dataset using PySpark.
Split the data into training and test sets based on the date "2011-09-25".
Encode categorical variables and train a Linear Regression model.
Calculate the MAE for the test set predictions.
Compute the total quantity sold for week 39 of 2011.


Outputs include:
pd_daily_train_data: A pandas DataFrame with the training set.
mae: The Mean Absolute Error of the model's predictions.
quantity_sold_w39: The total units sold in week 39 of 2011.



Dataset
The OnlineRetail.csv dataset contains e-commerce transaction data with the following columns:

InvoiceNo: Unique 6-digit transaction ID.
StockCode: Unique 5-digit product ID.
Description: Product name.
Quantity: Quantity of each product per transaction.
UnitPrice: Price per unit.
CustomerID: Unique 5-digit customer ID.
Country: Customer's country.
InvoiceDate: Transaction date and time.

File Structure

final_hw_a39948.ipynb: Jupyter notebook containing the analysis and forecasting logic.
OnlineRetail.csv: Input dataset (not included; must be provided).
requirements.txt: Lists required Python libraries.
README.md: This documentation file.

Dependencies
See requirements.txt for the list of required Python libraries.
Notes

The dataset is cleaned by removing rows with missing CustomerID or Description and filtering out negative Quantity values.
The InvoiceDate column is converted from string to date format for accurate splitting and analysis.
The Linear Regression model uses encoded Country and StockCode as features, which may limit predictive power due to the simplicity of the model. Consider experimenting with additional features or more complex models (e.g., Random Forest, XGBoost) for better performance.
The notebook checks for duplicate rows and finds none that are unreasonable, so all data is retained.
The calculation for week 39 of 2011 may return zero if no data exists for that week in the test set. Nearby weeks (38 and 40) are checked for context.

Troubleshooting

Missing dataset: Ensure OnlineRetail.csv is in the correct directory or update the file path in the notebook.
PySpark errors: Verify that Spark is installed and configured correctly. Check SPARK_HOME and Python compatibility.
Zero quantity for week 39: This may occur if the test set lacks data for week 39. Verify the date range and week calculations.
High MAE: The Linear Regression model may not capture complex patterns. Consider feature engineering or alternative models.

License
This project is for educational purposes and provided as-is without any warranty.
