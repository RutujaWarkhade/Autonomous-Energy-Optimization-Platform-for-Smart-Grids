# ⚡ Autonomous Energy Optimization Platform for Smart Grids

**🚀 Live Demo:** [https://autonomous-energy-optimization-platform.onrender.com/](https://autonomous-energy-optimization-platform.onrender.com/)


---

## 📊 Overview

This project turns raw smart meter + weather data into a full analytics product:

- **Data pipeline** — cleaning, merging, and feature engineering on ~3.5M household-days of smart meter readings and weather data.
- **Forecasting model** — a trained LightGBM model that predicts next-day (and 7-day-ahead) household energy consumption.
- **Household segmentation** — K-Means clustering that groups households into behavioral tiers (High Consumption, Peak-Time Users, Medium Consumption).
- **Interactive dashboard** — a Flask web app with live charts, a "predict your own consumption" tool, usage-pattern breakdowns, and AI-generated optimization recommendations.

---

## 🧠 Model Performance

Multiple models were trained and compared on a chronological train/validation/test split. The best-performing model was selected for deployment:

| Model | MAE | RMSE | R² |
|---|---|---|---|
| **LightGBM (selected)** | 2.16 | 3.96 | **0.844** |
| XGBoost | 2.16 | 3.97 | 0.844 |
| Decision Tree | 2.19 | 4.01 | 0.841 |
| Linear Regression | 2.37 | 4.13 | 0.831 |
| Persistence Baseline (Lag_1) | 2.55 | 4.44 | 0.805 |

The final model (LightGBM, early-stopped at 684 trees) is used to power both the historical accuracy charts and the live "Predict Energy Consumption" tool on the dashboard.

---

## 🏠 Household Segments

Using silhouette-selected K-Means clustering (k=3) on usage behavior, households were grouped into:

- **High Consumption** — highest-volume, steady usage households; the biggest lever for demand-response programs.
- **Peak-Time Users** — sharp weekday spikes and the most volatile day-to-day usage; prime candidates for load-shifting.
- **Medium Consumption** — moderate, weekend-leaning usage with room to shift a small share of load.

---

## ✨ Features

- 📈 **Tomorrow's forecast** — history-to-forecast chart with an 80% confidence band, plus a 7-day-ahead recursive forecast.
- 🔮 **Live prediction tool** — enter household, calendar, and weather conditions and get an instant model-driven consumption estimate.
- 🕒 **Usage pattern analysis** — hourly load shape, weekday vs. weekend comparison, peak vs. off-peak split, and monthly trends.
- 👥 **Household segmentation view** — cluster distribution and per-segment behavioral summaries.
- 💡 **AI optimization insights** — data-driven, plain-language recommendations for reducing peak load and improving efficiency.
- 🌦️ **Weather correlation** — visualizes the relationship between temperature and energy demand.

---

## 🛠️ Tech Stack

| Layer | Tools |
|---|---|
| Data processing & modeling | Python, Pandas, NumPy, Scikit-learn, LightGBM/XGBoost, Joblib |
| Backend | Flask, Gunicorn |
| Frontend | HTML, CSS, JavaScript, Chart.js |
| Deployment | Render |

---

## 📁 Project Structure

```
├── app.py                          # Flask application & prediction API
├── requirements.txt                # Python dependencies
├── notebooks/
│   ├── 01_Data_Preprocessing.ipynb # Data cleaning, merging, feature engineering
│   └── 02_Forecasting_Model.ipynb  # EDA, clustering, model training & evaluation
├── models/
│   ├── energy_forecasting_model.pkl        # Trained LightGBM model
│   ├── tabular_feature_scaler.pkl          # Feature scaler
│   ├── predictions.csv                     # Test-set actual vs. predicted values
│   └── forward_forecast_next_7_days.csv    # 7-day-ahead recursive forecast
├── templates/
│   └── index.html                  # Dashboard UI
└── static/
    ├── css/style.css
    └── js/main.js
```

---

## ⚙️ Local Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/<your-username>/<your-repo>.git
   cd <your-repo>
   ```

2. **Create a virtual environment & install dependencies**
   ```bash
   python -m venv venv
   source venv/bin/activate   # On Windows: venv\Scripts\activate
   pip install -r requirements.txt
   ```

3. **Run the app**
   ```bash
   python app.py
   ```

4. Open your browser at `http://localhost:5000`

---

## ☁️ Deployment

This project is deployed on **[Render](https://render.com/)** as a web service:

- **Build command:** `pip install -r requirements.txt`
- **Start command:** `gunicorn app:app`
- The app reads the `PORT` environment variable (provided automatically by Render) to bind the server.

**Live app:** [https://autonomous-energy-optimization-platform.onrender.com/](https://autonomous-energy-optimization-platform.onrender.com/)

---

## 📓 Notebooks

- **`01_Data_Preprocessing.ipynb`** — loads and merges the smart meter and weather datasets, performs data quality checks and cleaning, memory optimization, exploratory analysis, and feature engineering (calendar features, lag features, rolling statistics).
- **`02_Forecasting_Model.ipynb`** — full modeling pipeline: EDA, K-Means usage segmentation, anomaly detection, feature selection/encoding, chronological train/validation/test split, model training & comparison (Linear Regression, Decision Tree, XGBoost, LightGBM), cross-validation, hyperparameter search, feature importance, and generation of the forward-looking 7-day forecast.

---

## 📌 Notes

- Predictions come from a trained LightGBM regression model with an 80% confidence interval derived from test-set RMSE.
- The hourly usage-pattern chart uses an illustrative, well-established residential double-peak demand curve (morning + evening peaks), since half-hourly raw meter readings aren't part of the saved model artifacts.
- Household segments and optimization insights are grounded in the notebook's actual clustering and correlation results.

---

## 📄 License

This project is open source and available for personal and educational use. Feel free to fork and adapt it.
