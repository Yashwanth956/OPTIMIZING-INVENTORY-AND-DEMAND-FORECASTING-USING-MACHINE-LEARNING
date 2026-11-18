# OPTIMIZING-INVENTORY-AND-DEMAND-FORECASTING-USING-MACHINE-LEARNING
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

## Interpreting the dashboard

The Random Forest demo uses day of year as its input. A nearly horizontal predicted-versus-actual plot means the model gives similar estimates across different actual sales values.

The reorder flag compares the sales value with the selected threshold. Actual purchasing decisions also require on-hand stock, supplier lead time, and the units being forecast.

## Project work

The original application was developed by a teammate. My contributions include running the project locally, updating the dashboard background, testing the demo datasets, and investigating the ARIMA forecasting error.
