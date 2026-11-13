# Big Data Pipeline with PySpark, Spark SQL, Hive and MLlib

End-to-end big-data workflow on customer transaction data, built in Google Colab for my MSc Data Analytics (BSBI, Berlin).

## What's inside
- `big_data_pipeline.py`: the full pipeline, exported from the Colab notebook

## Pipeline
1. **Load**: read the transactions CSV into a Spark DataFrame with schema inference
2. **Clean**: drop rows missing key fields (year, month, amount)
3. **Analyse with Spark SQL**: monthly spending trends, spend per expense type, top 5 customers
4. **Model with MLlib**: `StringIndexer` + `VectorAssembler` pipeline, then a Random Forest regressor (50 trees) to predict transaction amount, evaluated with RMSE and R²
5. **Explain**: feature-importance chart
6. **Benchmark**: compared HiveQL-style queries with the DataFrame API on execution time

## How to run
Open it in Google Colab, run `!pip install pyspark`, upload the transactions CSV, and set `file_path` to its location.

**Tools:** PySpark, Spark SQL, Hive, Spark MLlib, Matplotlib, Google Colab
