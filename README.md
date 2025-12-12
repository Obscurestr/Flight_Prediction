# 1.Executive Summary
This report analyzes factors that influence airline ticket prices and builds a predictive model that estimates future prices based on historical data.

The analysis shows that flight price is primarily determined by:
- Number of stops
- Flight duration
- Airline
- Destination
- Month & time of travel

The final model (Random Forest Regressor) provides accurate price predictions and can be integrated into pricing tools, comparison apps, or revenue-management systems.
# 2. Dataset Overview
This dataset contains flight booking information including airline, journey date, departure time, arrival time, route, flight duration, total stops, and ticket price. It includes several major Indian airlines and covers both direct and connecting flights.
<img width="1114" height="464" alt="Screenshot 2025-12-12 182141" src="https://github.com/user-attachments/assets/96a3d8b5-cc3f-4430-b23d-e8141d28ba36" />

## Key characteristics:
- ~10,000 flight records
- Columns include Airline, Price, Departure Time, Arrival Time, Route, Duration, Stops, Source, Destination and Additional Information.
- After cleaning, there are no missing values.
<img width="266" height="519" alt="Screenshot 2025-12-12 183205" src="https://github.com/user-attachments/assets/1535421e-e497-4227-a409-c5aced59751d" />

# 3. Data Cleaning & Preparation

## 3.1 Converting Dates & Times
We converted timestamps into structured components:
- Journey Day
- Journey Month
- Journey Year
- Departure hour & minute
- Arrival hour & minute

This allows us to understand when people travel and how timing influences price.

<img width="1678" height="162" alt="Screenshot 2025-12-12 185613" src="https://github.com/user-attachments/assets/dce43cfe-93a8-4bc2-bb20-5cf0af6a90ba" />

## 3.2 Categorizing Departure Time
We grouped departure hours into categories consumers understand:
-Early Morning
- Morning
- Noon
- Evening
- Night
- Late Night

Used for identifying expensive travel windows.

<img width="560" height="517" alt="image" src="https://github.com/user-attachments/assets/e612958f-e2c2-4a56-9b3d-7b6d13b54bb5" />

## 3.3 Duration Preprocessing
Duration appears in inconsistent formats (“2h”, “50m”, “3h 10m”).

We standardized each entry and extracted:
- Duration in hours
- Duration in minutes
- Total duration in minutes
- 
<img width="1689" height="164" alt="Screenshot 2025-12-12 190747" src="https://github.com/user-attachments/assets/ae157b04-c521-409c-95ee-e6925a0adecd" />

# 4. Exploratory Data Analysis (EDA)
## 4.1 Price vs. Total Duration

Shows how longer trips generally cost more.

<img width="606" height="433" alt="image" src="https://github.com/user-attachments/assets/81461f58-1121-41a8-aa8e-1d4a8314b0c4" />

As flight duration increases, ticket prices also tend to increase, but not always linearly because other factors like airline and stops impact pricing.
## 4.2 Price vs. Duration by Number of Stops

Shows stop count influence on price clusters.

<img width="606" height="433" alt="image" src="https://github.com/user-attachments/assets/5eb186ba-c298-4028-b2bb-b4a8e122f8c9" />

- Non-stop flights are the most expensive.
- 1-stop flights are cheaper.
- 2+ stops are usually budget flights.

## 4.3 Airline Price Distribution

Demonstrates which airlines typically charge more.

<img width="589" height="665" alt="image" src="https://github.com/user-attachments/assets/16486b50-81e7-43b7-9302-a1cff4914dad" />

This chart shows the typical price ranges for each airline. Some carriers have consistently higher prices, suggesting a premium service.

# 5. Encoding Categorical Variables
## 5.1 Airline Encoding

Airlines were converted into numerical values based on average price.

<img width="416" height="274" alt="Screenshot 2025-12-12 192731" src="https://github.com/user-attachments/assets/391e5537-9f68-4b85-abbc-4dc40aa928a9" />

This numeric conversion allows the machine-learning model to recognize which airlines are premium and which are budget-friendly.
## 5.2 Destination Encoding

We unified “New Delhi” → “Delhi” and encoded destinations based on average price.

<img width="695" height="29" alt="Screenshot 2025-12-12 193609" src="https://github.com/user-attachments/assets/77f52729-90ac-4de2-bde9-3062a3404c6f" />

## 5.3 Total Stops

Mapped to numbers for modeling:

non-stop → 0

1 stop → 1  

2 stops → 2 

3 stops → 3  

4 stops → 4  
This lets our model understand that more stops usually mean cheaper tickets.
# 6. Outlier Treatment
## 6.1 Identifying Outliers
We used the IQR method:

Q1 = 5277.0

Q3 = 12373.0

IQR = 7096.0

Lower bound = -5367.0

Upper bound = 23017.0

## 6.2 Handling Outliers

Prices greater than 35,000 were replaced with the median price.

<img width="606" height="432" alt="image" src="https://github.com/user-attachments/assets/de246a63-cb79-405f-a159-31d7436a240b" />

This prevents unusually high prices from distorting the model.
# 7. Feature Importance

We used Mutual Information Regression to identify the most influential features.

<img width="1688" height="453" alt="Screenshot 2025-12-12 195203" src="https://github.com/user-attachments/assets/6d943c28-1c3a-4fef-ac95-401e31f94dfe" />

Typical results:
1. Total Stops
2. Duration Total Minutes
3. Airline
4. Destination
5. Journey Month

# 8. Model Building
## 8.1 Random Forest Regressor

The main model used for prediction.
R² Score: 0.8188320914090159

MAE: 1166.2465289582558

MSE: 3479097.837327007

RMSE: 1865.2339899666763

MAPE: 13.135809518140947

R² close to 1.0 means very good predictive accuracy.

MAPE < 15% means excellent reliability for pricing.

## 8.2 Decision Tree Regressor (Baseline)
R²: 0.6949885058427925

MAE: 1387.4563055397996

MAPE: 15.442938513427615

This simple model performs worse than Random Forest and confirms the value of using an advanced ensemble model.

# 9. Hyperparameter Optimization

We improved the model with RandomizedSearchCV.

<img width="1006" height="113" alt="Screenshot 2025-12-12 202212" src="https://github.com/user-attachments/assets/fa67f3b9-05d7-427a-8d34-4a03c6674ec5" />

We tested many combinations of model settings to find the best-performing configuration for your data.
# 10. Final Model Export

The final optimized model is saved as:

rf_random.pk1

This can be loaded into:
- Web apps
- Prediction dashboards
- Mobile apps
- Backend APIs
