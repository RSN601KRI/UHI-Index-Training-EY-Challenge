# Urban Heat Island (UHI) Prediction Model  
**EY Open Science AI & Data Challenge 2025**  
*A machine learning model to predict UHI hotspots in NYC using satellite, building, and weather data.*

---

## 📌 Overview  
This project predicts Urban Heat Island (UHI) Index values for urban locations in New York City, focusing on the Bronx and Manhattan regions. The model uses **satellite imagery**, **building footprints**, and **weather data** to identify heat hotspots while adhering to challenge constraints (no latitude/longitude features).

---

## 🚀 Key Features  
- **Data Sources**:  
  - Satellite: Sentinel-2 (Microsoft Planetary Computer)  
  - Building Footprints: NYC Open Data  
  - Weather: NYS Mesonet  
- **Features**: NDVI, Building Density, Temperature, Wind Speed.  
- **Model**: XGBoost Regressor (RMSE: ~0.15).  
- **Compliance**: No latitude/longitude-derived features used.  

---

## 🛠️ Installation  
1. **Clone the repository**:  
   ```bash
   git clone https://github.com/yourusername/uhi-prediction.git
   cd uhi-prediction
   ```

2. **Install dependencies**:  
   ```bash
   pip install pystac-client planetary-computer rasterio geopandas xgboost shap pandas numpy
   ```

3. **Set up API access**:  
   - Sign up for [Microsoft Planetary Computer](https://planetarycomputer.microsoft.com/) for satellite data access.  
   - Download building footprints from [NYC Open Data](https://data.cityofnewyork.us/Housing-Development/Building-Footprints/nqwf-w8eh).  

---

## 📂 Dataset Preparation  
1. **Training Data**: `Training_data_uhi_index_2025-02-18.csv` (provided by EY).  
2. **Satellite Data**:  
   - Sentinel-2 imagery (July 24, 2021) via STAC API.  
   - Extracted NDVI, NDBI, and albedo using `rasterio`.  
3. **Building Footprints**:  
   - Calculated building density within 100m buffers.  
4. **Weather Data**:  
   - Aggregated temperature, humidity, and wind speed from NYS Mesonet.  

---

## 🧠 Model Training  
```python
import xgboost as xgb

# Train XGBoost model
model = xgb.XGBRegressor(objective='reg:squarederror')
model.fit(X_train, y_train)

# Evaluate
y_pred = model.predict(X_test)
print(f"RMSE: {mean_squared_error(y_test, y_pred, squared=False)}")

## 📊 Results  
- **Top Contributors to UHI**:  
  1. Low vegetation (NDVI)  
  2. High building density  
  3. Low wind speed  
- **SHAP Analysis**:  
  ![SHAP Feature Importance](images/shap_plot.png)  

---

## 📤 Submission  
1. Generate predictions for validation data:  
   ```python
   validation_data['Predicted_UHI_Index'] = model.predict(validation_data[features])
   validation_data[['Longitude', 'Latitude', 'Predicted_UHI_Index']].to_csv('submission.csv', index=False)
   ```
2. Upload `submission.csv` to the challenge platform.  

## 📜 License & References  
- **Sentinel-2 Data**: [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)  
- **Building Footprints**: [NYC Open Data Terms](https://www.nyc.gov/home/terms-of-use.page)  
- **Challenge Guidelines**: [EY Open Science 2025](https://www.ey.com/en_gl/open-science)  

**Note**: Latitude and longitude are used only for data alignment, not as model features.  
