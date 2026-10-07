# 🌦️ Meteorological Disaster Monitoring System

A real-time **Meteorological Disaster Monitoring System** that combines weather data, disaster-risk analysis, machine learning, interactive maps, weather forecasting, and automated alerts into a single monitoring platform.

The project is designed to help users monitor potentially hazardous meteorological conditions such as **extreme heat, heavy rainfall, strong winds, low atmospheric pressure, thunderstorms, and tropical storms/cyclones**.

---

## 🚀 Project Overview

Extreme weather events can cause significant damage to infrastructure, agriculture, transportation, and human life. Early identification of dangerous weather conditions can help authorities and individuals take preventive action.

This project provides a centralized monitoring system that:

- 🌡️ Collects real-time weather information
- 🌧️ Monitors rainfall and precipitation
- 💨 Tracks wind speed and direction
- 🌪️ Monitors publicly available tropical storm/cyclone information
- 📊 Calculates a meteorological disaster-risk score
- 🤖 Uses Machine Learning for risk-level classification
- 🚨 Generates automatic disaster alerts
- 🗺️ Displays risk information on an interactive map
- 📈 Visualizes weather forecasts
- 📄 Generates downloadable monitoring reports
- 🖥️ Provides an interactive web dashboard through Gradio

---

# ✨ Key Features

## 1. 🌦️ Real-Time Weather Monitoring

The system retrieves current weather information including:

- Temperature
- Feels-like temperature
- Relative humidity
- Rainfall
- Wind speed
- Wind direction
- Atmospheric pressure
- Current weather condition

The system uses the **Open-Meteo API** for real-time weather and forecast information.

---

## 2. 🚨 Disaster Risk Assessment

The system evaluates multiple meteorological parameters and generates a risk score from:

```text
0 – 100
```

The risk level is categorized as:

| Risk Score | Risk Level |
|---:|---|
| 0–19 | 🟢 LOW |
| 20–44 | 🟠 MODERATE |
| 45–69 | 🟠 HIGH |
| 70–100 | 🔴 CRITICAL |

The risk engine considers:

- Extreme temperature
- Heavy rainfall
- Extreme rainfall
- Strong winds
- Extreme winds
- High humidity
- Low atmospheric pressure

---

## 3. 🧠 Machine Learning Risk Prediction

A Random Forest classifier is integrated into the system.

The ML model considers:

```text
Temperature
Humidity
Rainfall
Wind Speed
Atmospheric Pressure
```

and predicts:

```text
LOW
MODERATE
HIGH
CRITICAL
```

### Important

The current notebook uses a **synthetically generated training dataset** based on meteorological thresholds.

This makes the project functional for demonstration and development, but the model should be retrained using historical meteorological and disaster datasets before being used for scientific forecasting or real-world emergency decisions.

---

# 4. 🌀 Cyclone / Storm Monitoring

The system attempts to retrieve active tropical storm information from publicly available **NOAA/National Hurricane Center** endpoints.

The storm monitoring module can identify information such as:

- Storm name
- Basin
- Active storm records

The system is designed so that failure of the public storm endpoint does not stop the rest of the monitoring application.

---

# 5. 🔑 Ambee API Integration

The architecture also supports optional integration with **Ambee APIs**.

Ambee can be used as an additional environmental/weather data source.

Add your API key:

```python
AMBEE_API_KEY = "YOUR_AMBEE_API_KEY"
```

The system can then access Ambee weather information through its API.

> An Ambee API key is not included in this repository. Users must obtain their own credentials from Ambee.

---

# 6. 🗺️ Interactive Disaster Risk Map

The project uses **Folium** to generate an interactive map.

The map displays:

- Monitoring location
- Current risk level
- Risk score
- Weather information
- Hazard information
- Risk-zone radius

Example conceptual structure:

```text
              🗺️ RISK MAP

                    🔴
               Risk Zone
                  /   \
                 /     \
                /  📍   \
               / Location\
              /           \
```

The generated map is saved as:

```text
meteorological_disaster_map.html
```

---

# 7. 📊 Weather Forecast Visualization

The system retrieves hourly forecast information and creates interactive Plotly visualizations.

The visualization can display:

- Temperature trends
- Rainfall
- Forecast timeline

The chart allows users to identify potentially dangerous changes in weather conditions.

---

# 8. 🚨 Automated Disaster Alerts

The system automatically generates alerts according to the detected risk.

### Low Risk

```text
🟢 LOW RISK
No major meteorological threat detected.
```

### Moderate Risk

```text
🟠 MODERATE RISK
Monitor weather conditions.
```

### High Risk

```text
⚠️ HIGH RISK ALERT
Precautionary measures recommended.
```

### Critical Risk

```text
🚨 CRITICAL DISASTER ALERT
Immediate attention required.
```

---

# 9. 📄 Automatic Report Generation

The system generates a CSV report containing:

```text
Location
Latitude
Longitude
Temperature
Feels Like Temperature
Humidity
Rainfall
Wind Speed
Wind Direction
Atmospheric Pressure
Weather Condition
Risk Score
Risk Level
ML Prediction
Detected Hazards
Timestamp
```

Generated file:

```text
meteorological_disaster_report.csv
```

---

# 🖥️ Interactive Dashboard

The project includes a **Gradio web dashboard**.

Users can enter a city such as:

```text
Bengaluru
```

or:

```text
Chennai
```

or:

```text
Hyderabad
```

The dashboard returns:

1. Current weather
2. Disaster risk score
3. Risk classification
4. ML prediction
5. Detected hazards
6. Forecast visualization
7. Interactive risk map
8. Monitoring report

---

# 🏗️ System Architecture

```text
                    ┌──────────────────────┐
                    │       USER           │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │  Gradio Dashboard    │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │ Location / Geocoding │
                    └──────────┬───────────┘
                               │
                ┌──────────────┼──────────────┐
                │              │              │
                ▼              ▼              ▼
        ┌──────────────┐ ┌────────────┐ ┌──────────────┐
        │ Open-Meteo   │ │ NOAA/NHC   │ │ Ambee API    │
        │ Weather API  │ │ Storm Data │ │ Optional     │
        └──────┬───────┘ └─────┬──────┘ └──────┬───────┘
               │               │               │
               └───────────────┼───────────────┘
                               ▼
                    ┌──────────────────────┐
                    │ Weather Data Engine  │
                    └──────────┬───────────┘
                               │
                ┌──────────────┴──────────────┐
                ▼                             ▼
       ┌─────────────────┐          ┌──────────────────┐
       │ Risk Assessment │          │ ML Risk Model    │
       │ Engine          │          │ Random Forest    │
       └────────┬────────┘          └────────┬─────────┘
                │                            │
                └──────────────┬─────────────┘
                               ▼
                    ┌──────────────────────┐
                    │ Disaster Risk Engine │
                    └──────────┬───────────┘
                               │
              ┌────────────────┼────────────────┐
              ▼                ▼                ▼
       ┌─────────────┐ ┌──────────────┐ ┌──────────────┐
       │ Alert       │ │ Risk Map     │ │ Forecast     │
       │ Generation  │ │ Folium       │ │ Visualization│
       └─────────────┘ └──────────────┘ └──────────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │ CSV Monitoring Report│
                    └──────────────────────┘
```

---

# 🛠️ Technology Stack

## Programming Language

- Python 3

## APIs

- Open-Meteo API
- NOAA/NHC public storm information
- Optional Ambee Weather API

## Data Processing

- Pandas
- NumPy

## Machine Learning

- Scikit-learn
- Random Forest
- StandardScaler

## Visualization

- Plotly
- Matplotlib
- Folium

## Dashboard

- Gradio

## Geospatial

- Folium
- GeoPandas
- Shapely
- PyProj

---

# 📦 Installation

The project is designed for **Google Colab**, so most dependencies can be installed automatically.

Run:

```bash
pip install requests pandas numpy matplotlib scikit-learn folium gradio plotly geopandas shapely pyproj fiona
```

---

# ▶️ Running the Project

## Option 1 — Google Colab

1. Open Google Colab.
2. Create a new notebook.
3. Copy the complete project code into a cell.
4. Run the cell.
5. Wait for the dependencies to install.
6. The system will perform an initial test using Bengaluru.
7. The Gradio dashboard will launch.
8. Open the generated Gradio public URL.

---

# 📍 Example

Enter:

```text
Chennai
```

The system performs:

```text
Chennai
   ↓
Geocoding
   ↓
Weather API
   ↓
Weather Parameters
   ↓
Risk Calculation
   ↓
Machine Learning Prediction
   ↓
Alert Generation
   ↓
Risk Map + Forecast
   ↓
CSV Report
```

---

# 📊 Example Output

```text
METEOROLOGICAL DISASTER MONITORING REPORT
==========================================

Location:
Chennai

Coordinates:
13.0827, 80.2707

CURRENT WEATHER
---------------
Condition       : Moderate rain
Temperature     : 29.4 °C
Feels Like      : 34.2 °C
Humidity        : 86 %
Rainfall        : 12.4 mm
Wind Speed      : 31.2 km/h
Wind Direction  : 142°
Pressure        : 1003 hPa

DISASTER ANALYSIS
-----------------
Risk Score      : 27/100
Risk Level      : MODERATE
ML Prediction   : MODERATE

Detected Hazards:
HEAVY HUMIDITY
BELOW NORMAL PRESSURE
```

*Values above are illustrative; actual values depend on live API data.*

---

# 📁 Generated Files

After execution, the project generates:

```text
meteorological_disaster_report.csv
meteorological_disaster_map.html
```

### CSV Report

Contains structured monitoring data that can be further analyzed using:

- Excel
- Power BI
- Tableau
- Python
- SQL

### HTML Map

The generated Folium map can be opened in a browser and shared as an HTML file.

---

# 🔐 API Configuration

## Open-Meteo

No API key is required for the basic implementation.

The project uses:

```text
https://api.open-meteo.com/
```

and the Open-Meteo geocoding service.

---

## Ambee

For optional Ambee integration:

```python
AMBEE_API_KEY = "YOUR_AMBEE_API_KEY"
```

Do **not** commit your actual API key to GitHub.

Instead, use environment variables:

```python
import os

AMBEE_API_KEY = os.getenv("AMBEE_API_KEY")
```

In Colab:

```python
import os

os.environ["AMBEE_API_KEY"] = "YOUR_API_KEY"
```

---

# 🧠 Machine Learning Pipeline

The current ML pipeline is:

```text
Weather Parameters
       │
       ▼
Data Preparation
       │
       ▼
StandardScaler
       │
       ▼
Random Forest Classifier
       │
       ▼
Risk Classification
       │
       ├── LOW
       ├── MODERATE
       ├── HIGH
       └── CRITICAL
```

### Input Features

```text
Temperature
Humidity
Rainfall
Wind Speed
Atmospheric Pressure
```

### Output

```text
Meteorological Risk Level
```

---

# ⚠️ Current Limitations

This project is a functional prototype and should not be treated as an operational emergency-management system.

### 1. Synthetic ML Training Data

The current Random Forest model is trained using generated data.

For production use, it should be trained using historical observations.

### 2. No Scientific Cyclone Prediction

The system monitors available storm information but does not perform advanced cyclone-track forecasting.

### 3. Public API Availability

External APIs can change their:

- endpoints
- rate limits
- response formats
- authentication requirements

### 4. No SMS/E-mail Gateway

The current alert system displays alerts inside the application.

A production system could integrate:

- SMS
- Email
- WhatsApp
- Push notifications

---

# 🚀 Future Enhancements

The project can be significantly expanded.

## 1. 🌀 Cyclone Prediction

Integrate historical cyclone-track datasets and develop models for:

- Cyclone formation
- Cyclone intensity
- Track prediction
- Landfall prediction

---

## 2. 🌊 Flood Prediction

Add:

- River-level data
- Rainfall accumulation
- Elevation data
- Drainage networks
- Soil moisture
- Historical flood records

Then develop:

```text
Flood Probability
+
Flood Severity
+
Flood Risk Map
```

---

## 3. 🔥 Heatwave Prediction

Develop a dedicated heatwave model using:

- Temperature
- Humidity
- Heat index
- Historical temperature
- Wind
- Atmospheric pressure

---

## 4. 🌧️ Extreme Rainfall Prediction

Train an ML model using historical rainfall records.

Possible algorithms:

```text
Random Forest
XGBoost
LightGBM
LSTM
GRU
```

---

## 5. 🛰️ Satellite Data

Integrate satellite imagery for:

- Cloud detection
- Cyclone structure
- Flood mapping
- Land-cover analysis
- Storm development

Possible sources include:

- Sentinel
- Landsat
- NASA datasets
- ISRO/public Indian datasets where available

---

## 6. 🗺️ Advanced Disaster-Risk Mapping

Add GIS layers such as:

```text
Population Density
Road Network
Hospitals
Schools
Rivers
Elevation
Flood Zones
Cyclone Paths
Emergency Shelters
```

This would transform the project into a more complete **disaster-management GIS platform**.

---

## 7. 📱 Mobile Application

Create a mobile application using:

```text
React Native
```

The application could provide:

- Live alerts
- Current location risk
- Emergency contacts
- Weather forecast
- Disaster map
- Evacuation routes

---

## 8. 🔔 Real-Time Notifications

Integrate:

```text
Twilio
Firebase Cloud Messaging
Email SMTP
WhatsApp Business API
```

to send emergency alerts.

Example:

```text
🚨 CRITICAL WEATHER ALERT

Heavy rainfall and strong winds
detected near your location.

Risk Level: CRITICAL
Risk Score: 82/100

Please follow local emergency guidance.
```

---

# 🔮 Proposed Production Architecture

A production version could use:

```text
                 Weather APIs
                     │
         ┌───────────┼───────────┐
         ▼           ▼           ▼
      Ambee       Open-Meteo    NOAA
         │           │           │
         └───────────┼───────────┘
                     ▼
              Data Ingestion
                     │
                     ▼
              Data Validation
                     │
                     ▼
             Feature Engineering
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
     ML Prediction        Rule Engine
          │                     │
          └──────────┬──────────┘
                     ▼
               Risk Engine
                     │
          ┌──────────┼──────────┐
          ▼          ▼          ▼
        Maps       Alerts     Dashboard
          │          │          │
          └──────────┼──────────┘
                     ▼
               End Users
```

---

# 🎯 Use Cases

This system can be useful for:

- Disaster management agencies
- Government authorities
- Municipal corporations
- Emergency response teams
- Smart-city platforms
- Agriculture monitoring
- Infrastructure monitoring
- Transportation management
- Insurance risk analysis
- Research institutions
- Educational projects

---

# 👨‍💻 Project Structure

A production version can be organized as:

```text
meteorological-disaster-monitoring/
│
├── README.md
│
├── notebook/
│   └── meteorological_disaster_monitoring.ipynb
│
├── data/
│   ├── raw/
│   ├── processed/
│   └── historical/
│
├── models/
│   └── disaster_risk_model.pkl
│
├── reports/
│   ├── meteorological_disaster_report.csv
│   └── meteorological_disaster_map.html
│
├── src/
│   ├── weather_api.py
│   ├── cyclone_monitor.py
│   ├── risk_engine.py
│   ├── ml_model.py
│   ├── alerts.py
│   └── mapping.py
│
├── dashboard/
│   └── app.py
│
└── requirements.txt
```

---

# 📚 APIs & Resources

- **Open-Meteo** — weather and forecast data
- **NOAA / National Hurricane Center** — tropical cyclone information
- **Ambee** — optional environmental and weather APIs
- **Folium** — interactive maps
- **Plotly** — interactive visualization
- **Scikit-learn** — machine learning

---

# ⚖️ Disclaimer

This project is intended for **educational, research, and software-development purposes**.

The generated risk score and ML predictions should **not be used as a substitute for official meteorological warnings, government emergency notifications, or professional disaster-management systems**.

For real-world deployment, the models should be validated against authoritative historical datasets and the system should incorporate official warnings from relevant meteorological and disaster-management authorities.

---

# 🌟 Future Project Goal

The long-term goal is to evolve this prototype into an intelligent platform capable of:

```text
MONITOR
   ↓
DETECT
   ↓
PREDICT
   ↓
ASSESS RISK
   ↓
MAP IMPACT
   ↓
GENERATE ALERT
   ↓
RECOMMEND ACTION
```

The final system could combine **IoT sensors + weather APIs + satellite imagery + GIS + machine learning + real-time alerts** to create a comprehensive meteorological disaster intelligence platform.

---

# 👨‍💻 Author

**Meteorological Disaster Monitoring System**

Developed as a Python-based data science, machine learning, GIS, and real-time weather monitoring project.

---

## ⭐ If You Find This Project Useful

Consider giving the repository a ⭐ and extending it with real historical disaster datasets, advanced ML models, satellite imagery, and real-time emergency notifications.
