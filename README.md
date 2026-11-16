# Big Data Pipeline with PySpark, Spark SQL and MLlib

End-to-end data workflow on customer transaction data, built in Google Colab for my MSc Data Analytics (BSBI, Berlin). It takes a transactions file from raw CSV to cleaned data, SQL analysis, a machine-learning model and a small performance comparison, all inside Apache Spark.

## What's inside

- `big_data_pipeline.py`: the full pipeline, exported from the Colab notebook

## Data

A subset of customer card transactions (`transactions_subset_2000.csv`). The pipeline uses these columns:

| Column | Meaning |
|--------|---------|
| `CUST_ID` | customer identifier |
| `YEAR`, `MONTH`, `DAY` | transaction date |
| `EXP_TYPE` | expense category |
| `AMOUNT` | transaction amount |

The data file is not included in this repository.

## Pipeline

1. **Load:** read the CSV into a Spark DataFrame with header and schema inference, then check the schema and first rows
2. **Clean:** drop rows where `YEAR`, `MONTH` or `AMOUNT` is missing, and keep everything else
3. **Analyse with Spark SQL:** register the data as a temporary view and query it for
   - total spend per month
   - number of transactions and total spend per expense type
   - the top 5 customers by total spend
4. **Prepare features:** an MLlib `Pipeline` with a `StringIndexer` (expense type to number) and a `VectorAssembler` (year, month, day, expense type)
5. **Model:** 80/20 train/test split, then a Random Forest regressor with 50 trees that predicts the transaction amount
6. **Evaluate:** RMSE and R² on the test set, and again on the training set to check for overfitting
7. **Explain:** bar chart of feature importances
8. **Compare query styles:** the same yearly aggregation written as a SQL query and with the DataFrame API, each timed

## A note on results

The saved notebook no longer contains its outputs, and the data file is not available to me any more, so this README documents the method and does not quote numbers. Running the script on the CSV prints all tables, the RMSE and R² values, and both charts.

## What I learned

- Setting up a Spark session and working with DataFrames in place of pandas
- Using Spark SQL for aggregation next to the DataFrame API
- Building an MLlib pipeline so that feature preparation is repeatable
- Comparing training and test scores to judge whether a model generalises

## Limitations and next steps

- The model only sees the date and the expense type, which says little about how large a single transaction will be. Customer-level features such as average past spend would be the first thing to add.
- The timing comparison runs each query once, and the first query also pays Spark's start-up cost. A fair benchmark would warm up first and average several runs. Both versions run on the same Spark engine, so large differences are not expected.
- The SQL query uses HiveQL-compatible syntax but runs on Spark SQL. No Hive cluster is involved.
- The dataset is a small subset. Spark only pays off at much larger volumes; the point here was to learn the tools.

## How to run

Open a new Google Colab notebook, run `!pip install pyspark`, upload the transactions CSV, set `file_path` to its location, and run the script.

**Tools:** PySpark, Spark SQL, Spark MLlib, Matplotlib, Google Colab
