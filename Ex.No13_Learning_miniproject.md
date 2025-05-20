# Ex.No: 10 Learning – Use Supervised Learning  
### DATE: 13/05/2025
### REGISTER NUMBER : 212222040075
### AIM: 
To write a program to train the classifier for Traffic Flow Forecasting Using LSTM Neural Networks.
###  Algorithm:

Step 1: Collect traffic volume data from various road junctions.
Step 2: Preprocess the data:
  Parse DateTime column to extract features: hour, day, month, year.
  Handle missing values.
  Filter data for a specific junction.
Step 3: Normalize the traffic volume using MinMaxScaler.
Step 4: Perform Exploratory Data Analysis (EDA):
  Plot traffic trends by hour, day, and month.
  Identify seasonal and temporal patterns.
Step 5: Prepare the dataset for LSTM:
  Use a sliding window with a look-back period (e.g., 24 hours).
  Create input-output pairs for supervised learning.
Step 6: Split the data into training (80%) and testing (20%) sets.
Step 7: Build the LSTM model:
  Stack two LSTM layers.
  Add dense output layer.
  Compile the model using mean squared error loss and Adam optimizer.
Step 8: Train the model:
  Use a small batch size (e.g., 1).
  Train for a few epochs (e.g., 3).
Step 9: Predict traffic volume on both training and testing sets.
Step 10: Evaluate model performance using metrics:
  MSE (Mean Squared Error)
  MAE (Mean Absolute Error)
  R² Score
  MAPE (Mean Absolute Percentage Error)
Step 11: Compare predictions with a baseline (persistence model).
Step 12: Visualize actual vs. predicted traffic volume.

### Program:
```
import pandas as pd
# Load the original dataset
df = pd.read_csv('/content/sample_data/traffic.csv')
# Check for missing values
missing_values = df.isnull().sum()
# Check data types
data_types = df.dtypes
# Check columns
columns = df.columns
# Display the information
print("Missing values in each column:\n", missing_values)
print("\nData types of each column:\n", data_types)
print("\nColumn names:\n", columns)

unique_junctions = df['Junction'].unique()
num_unique_junctions = len(unique_junctions)
# Display the number of unique junctions and their names
print("Number of unique junctions:", num_unique_junctions)
print("Unique junctions:\n", unique_junctions)

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
# Convert the DateTime column to datetime format
df['DateTime'] = pd.to_datetime(df['DateTime'], errors='coerce')
# Extract time-related features
df['hour'] = df['DateTime'].dt.hour
df['day_of_week'] = df['DateTime'].dt.dayofweek
df['month'] = df['DateTime'].dt.month
df['year'] = df['DateTime'].dt.year
# Get the unique junctions
unique_junctions = df['Junction'].unique()
# Perform EDA for each junction
for junction in unique_junctions:
    print(f"\nEDA for Junction {junction}")
    # Filter data for the junction
    df_junction = df[df['Junction'] == junction]
    # Traffic patterns by hour of the day
    plt.figure(figsize=(12, 6))
    df_junction.groupby('hour')['Vehicles'].mean().plot(kind='bar')
    plt.title(f'Average Traffic Volume by Hour of the Day - Junction {junction}')
    plt.xlabel('Hour of the Day')
    plt.ylabel('Average Number of Vehicles')
    plt.show()
    # Traffic patterns by day of the week
    plt.figure(figsize=(12, 6))
    df_junction.groupby('day_of_week')['Vehicles'].mean().plot(kind='bar')
    plt.title(f'Average Traffic Volume by Day of the Week - Junction {junction}')
    plt.xlabel('Day of the Week')
    plt.ylabel('Average Number of Vehicles')
    plt.xticks(ticks=np.arange(7), labels=['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'])
    plt.show()
    # Traffic patterns by month
    plt.figure(figsize=(12, 6))
    df_junction.groupby('month')['Vehicles'].mean().plot(kind='bar')
    plt.title(f'Average Traffic Volume by Month - Junction {junction}')
    plt.xlabel('Month')
    plt.ylabel('Average Number of Vehicles')
    plt.xticks(ticks=np.arange(1, 13), labels=['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'])
    plt.show()
    # Time series plot for the junction
    plt.figure(figsize=(15, 6))
    plt.plot(df_junction['DateTime'], df_junction['Vehicles'], label=f'Junction {junction}')
    plt.title(f'Traffic Volume Over Time - Junction {junction}')
    plt.xlabel('DateTime')
    plt.ylabel('Number of Vehicles')
    plt.legend()
    plt.show()

df['hour'] = df['DateTime'].dt.hour
df['day_of_week'] = df['DateTime'].dt.dayofweek
df['month'] = df['DateTime'].dt.month
df['year'] = df['DateTime'].dt.year
# Traffic patterns by hour of the day (combined junctions)
plt.figure(figsize=(12, 6))
df.groupby('hour')['Vehicles'].mean().plot(kind='bar')
plt.title('Average Traffic Volume by Hour of the Day (Combined Junctions)')
plt.xlabel('Hour of the Day')
plt.ylabel('Average Number of Vehicles')
plt.show()
# Traffic patterns by day of the week (combined junctions)
plt.figure(figsize=(12, 6))
df.groupby('day_of_week')['Vehicles'].mean().plot(kind='bar')
plt.title('Average Traffic Volume by Day of the Week (Combined Junctions)')
plt.xlabel('Day of the Week')
plt.ylabel('Average Number of Vehicles')
plt.xticks(ticks=np.arange(7), labels=['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'])
plt.show()
# Traffic patterns by month (combined junctions)
plt.figure(figsize=(12, 6))
df.groupby('month')['Vehicles'].mean().plot(kind='bar')
plt.title('Average Traffic Volume by Month (Combined Junctions)')
plt.xlabel('Month')
plt.ylabel('Average Number of Vehicles')
plt.xticks(ticks=np.arange(1, 13), labels=['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'])
plt.show()
# Time series plot for combined junctions
plt.figure(figsize=(15, 6))
for junction in df['Junction'].unique():
    plt.plot(df[df['Junction'] == junction]['DateTime'], df[df['Junction'] == junction]['Vehicles'], label=f'Junction {junction}')
plt.title('Traffic Volume Over Time (Combined Junctions)')
plt.xlabel('DateTime')
plt.ylabel('Number of Vehicles')
plt.legend()
plt.show()

import numpy as np
from sklearn.preprocessing import MinMaxScaler
from keras.models import Sequential
from keras.layers import Dense, LSTM
from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score, mean_absolute_percentage_error
import pandas as pd
import matplotlib.pyplot as plt

# Load the dataset
file_path = '/content/sample_data/traffic.csv'
df = pd.read_csv(file_path)
# Convert the DateTime column to datetime format
df['DateTime'] = pd.to_datetime(df['DateTime'], errors='coerce')
# Set DateTime as the index
df.set_index('DateTime', inplace=True)
# Select data for a specific junction
junction = 1
df_junction = df[df['Junction'] == junction]['Vehicles']
# Prepare data for LSTM
scaler = MinMaxScaler(feature_range=(0, 1))
df_junction_scaled = scaler.fit_transform(df_junction.values.reshape(-1, 1))
# Split data into training and testing sets
train_size = int(len(df_junction_scaled) * 0.8)
train, test = df_junction_scaled[:train_size], df_junction_scaled[train_size:]
# Create dataset function
def create_dataset(dataset, look_back=1):
    X, y = [], []
    for i in range(len(dataset) - look_back - 1):
        X.append(dataset[i:(i + look_back), 0])
        y.append(dataset[i + look_back, 0])
    return np.array(X), np.array(y)
look_back = 24
X_train, y_train = create_dataset(train, look_back)
X_test, y_test = create_dataset(test, look_back)
# Reshape input to be [samples, time steps, features]
X_train = np.reshape(X_train, (X_train.shape[0], X_train.shape[1], 1))
X_test = np.reshape(X_test, (X_test.shape[0], X_test.shape[1], 1))
# Build the LSTM model
model = Sequential()
model.add(LSTM(50, return_sequences=True, input_shape=(look_back, 1)))
model.add(LSTM(50, return_sequences=False))
model.add(Dense(25))
model.add(Dense(1))
model.compile(optimizer='adam', loss='mean_squared_error')
# Train the model
model.fit(X_train, y_train, batch_size=1, epochs=3)
# Make predictions
train_predict = model.predict(X_train)
test_predict = model.predict(X_test)
# Invert predictions
train_predict = scaler.inverse_transform(train_predict)
y_train = scaler.inverse_transform([y_train])
test_predict = scaler.inverse_transform(test_predict)
y_test = scaler.inverse_transform([y_test])
# Create a new dataframe to align the predictions with the dates
train_predict_plot = np.empty_like(df_junction_scaled)
train_predict_plot[:, :] = np.nan
train_predict_plot[look_back:len(train_predict) + look_back, :] = train_predict
test_predict_plot = np.empty_like(df_junction_scaled)
test_predict_plot[:, :] = np.nan
test_predict_plot[len(train_predict) + (look_back * 2) + 1:len(df_junction_scaled) - 1, :] = test_predict
# Plot LSTM forecast
plt.figure(figsize=(12, 6))
plt.plot(scaler.inverse_transform(df_junction_scaled), label='Actual Data')
plt.plot(train_predict_plot, label='LSTM Training Forecast')
plt.plot(test_predict_plot, label='LSTM Test Forecast')
plt.title('LSTM Forecast for Junction 1')
plt.xlabel('DateTime')
plt.ylabel('Number of Vehicles')
plt.legend()
plt.show()
# Performance evaluation
train_mse = mean_squared_error(y_train[0], train_predict)
train_mae = mean_absolute_error(y_train[0], train_predict)
test_mse = mean_squared_error(y_test[0], test_predict)
test_mae = mean_absolute_error(y_test[0], test_predict)
# Calculate R² score and MAPE for additional accuracy evaluation
train_r2 = r2_score(y_train[0], train_predict)
test_r2 = r2_score(y_test[0], test_predict)
train_mape = mean_absolute_percentage_error(y_train[0], train_predict)
test_mape = mean_absolute_percentage_error(y_test[0], test_predict)
print(f'Train MSE: {train_mse}')
print(f'Train MAE: {train_mae}')
print(f'Train R²: {train_r2}')
print(f'Train MAPE: {train_mape}')
print(f'Test MSE: {test_mse}')
print(f'Test MAE: {test_mae}')
print(f'Test R²: {test_r2}')
print(f'Test MAPE: {test_mape}')

```

### Output:

![1](https://github.com/user-attachments/assets/2a7937eb-39b7-49a4-b9af-21c5e777e67b)

![2](https://github.com/user-attachments/assets/8d9267c8-7768-4878-aa0e-5a8f4263f9f4)

![3](https://github.com/user-attachments/assets/6e4d002d-e29a-4402-9fb2-db2ece0e6088)

![4](https://github.com/user-attachments/assets/6b0b7fae-0cec-403e-87a8-c71e2144a99d)

![5](https://github.com/user-attachments/assets/e0549022-3883-4b8a-b7fd-1ac0978d4857)

### Result:
Thus the system was trained successfully and the prediction was carried out.
