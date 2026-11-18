# Inventory Optimization and Demand Forecasting

A Streamlit dashboard for exploring sales records, comparing predictions with historical sales, and reviewing reorder flags. Upload a CSV or Excel workbook to start.

## Run locally

Download [the application ZIP](ML_project.zip), extract it, and open a terminal in the folder containing `ML_project.py` and `background.png`. On Windows with Python 3.12:

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\python.exe -m pip install streamlit pandas numpy scikit-learn statsmodels tensorflow keras plotly chardet openpyxl
.\.venv\Scripts\python.exe -m streamlit run ML_project.py
```

Open the local URL printed in the terminal and create a demo account.

## Try the car sales demo

1. Download `sales_cars.csv` from the [demo datasets folder](datasets/).
2. In the dashboard, choose **App**, upload the CSV, and click **Evaluate Data**.
3. Find **Date Processing**. The app reads the purchase date and converts it to the day of the year. For example, 25 February 2024 becomes day 56.
4. Choose **Random Forest** and inspect **Predicted vs Actual Sales**. Hover over a point to compare the recorded sale with the prediction.

## Demo datasets

| File | Contents |
| --- | --- |
| `sales_cars.csv` | Car sales with purchase dates |
| `Sample - Superstore.xlsx` | Retail transactions with order dates |
| `Inventory_data.xlsx` | Inventory records by store, brand, and vendor |

## How the project works

### 1. Upload and inspect the data

The Streamlit dashboard accepts CSV and Excel files. It previews the uploaded rows so users can check what each record represents. In the car sales demo, the first visible record has a purchase date of 25 February 2024 and a sales value of 620. The dashboard processes each dataset separately.

### 2. Prepare dates and sales values

The app looks for a sales column and a date column, such as `purchase date` in the car dataset or `Order Date` in the Superstore workbook. It converts dates into a numeric feature called `DayOfYear`. For the example above, the Random Forest receives 56 as its input and 620 as the recorded sales value it is trying to predict. Other columns, such as income and mileage, appear in the preview but are excluded from this model's inputs.

### 3. Train a forecasting model

| Model | How the app uses it |
| --- | --- |
| Linear Regression | Fits a straight line between day of year and the recorded sales value. This provides a simple starting point for comparison. |
| Random Forest | Trains 100 decision trees on day of year and averages their predictions. The car sales demo uses this option. |
| ARIMA | Uses a sequence of sales observations to estimate future values. Testing on Superstore orders exposed an issue with repeated and out-of-order dates. In the corrected local version, sales are totaled by day, sorted by date, and missing calendar days are treated as zero sales. |
| LSTM | Scales sales values and uses sequences of past observations to train a neural network that predicts the next value. |

### 4. Read the results

For Linear Regression and Random Forest, the app randomly assigns 80% of the records to training and 20% to testing. The **Predicted vs Actual Sales** chart plots recorded sales horizontally and predicted sales vertically. In the car sales demo, the predictions formed a nearly horizontal band. The model returned similar values for transactions with very different sales. This illustrates a limitation of using day of year as the only input.

The random split measures performance on held-out records. To assess performance on future dates, train on earlier records and evaluate on a later period. The corrected local ARIMA workflow uses this chronological split.

### 5. Explore future sales and reorder flags

The forecast slider lets users select 1 to 30 days and download the resulting estimates as a CSV. Forecast values follow the meaning of the uploaded sales column. A forecast based on sales revenue is expressed in money. Estimating the number of items required calls for historical quantities sold.

The inventory section applies a separate threshold rule. With the slider set to 159, a sales value of 120 receives the label **Reorder**, while a value of 200 receives **Sufficient**. This demonstrates how the interface flags records. A purchasing decision would also require current stock, outstanding orders, and supplier lead time.
