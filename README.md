BahaWatch AI

Final project for the Building AI course

Summary

BahaWatch AI is a mobile-friendly tool that predicts hyper-local street flooding up to two hours in advance. It combines live weather radar, local drainage data, and community photos to warn commuters and residents before roads become impassable.


Background

Sudden heavy rainfall often causes rapid flash flooding on city streets, trapping commuters, damaging vehicles, and delaying emergency services. In tropical urban areas like Metro Manila, this happens dozens of times every year during the monsoon and typhoon seasons. Having personally experienced the frustration and danger of getting stranded in sudden floodwaters, I wanted to design a street-level warning system rather than relying on broad city-wide alerts. This topic is increasingly important because climate change is making extreme weather more frequent, and giving everyday people actionable data helps communities stay safe and avoid costly traffic gridlock.

How is it used?

Commuters, delivery riders, and public transport drivers check the app before or during heavy rain to choose safe routes, while local disaster response teams use the dashboard to deploy rescue vehicles proactively. For commuters, the app provides peace of mind and safer travel. However, local shop owners could lose customers if their street is flagged, making high accuracy essential to prevent damaging false alarms. Meanwhile, city officials might feel pressured by public visibility of poor drainage, though the data ultimately helps them prioritize infrastructure repairs.


This is how you create code examples:
import pandas as pd
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

# 1. Simulated dataset of past weather and street conditions
# Features: rainfall (mm/hr), street_elevation (meters), tide_level (meters), drainage_clogged (0=No, 1=Yes)
# Target: flooded (0 = Passable, 1 = Flooded)
data = {
    'rainfall_mm':,
    'elevation_m':,
    'tide_level_m':     [0.5, 1.8, 0.7, 2.0, 1.5, 0.4, 1.9, 1.0, 2.1, 0.6, 1.6, 0.5],
    'drainage_clogged':,
    'flooded':          [0,  1,  0,  1,  1,  0,  1,  0,  1,  0,  1,  0]
}

df = pd.DataFrame(data)

# 2. Separate features (X) and target label (y)
X = df[['rainfall_mm', 'elevation_m', 'tide_level_m', 'drainage_clogged']]
y = df['flooded']

# 3. Split data into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25, random_state=42)

# 4. Train the AI model (Random Forest Classifier)
model = RandomForestClassifier(n_estimators=100, random_state=42)
model.fit(X_train, y_train)

# 5. Predict flood risk for a new, unseen street condition
# Example: 48 mm/hr rain, low elevation (2m), high tide (1.7m), clogged drain (1)
new_street = pd.DataFrame([[48, 2, 1.7, 1]], columns=X.columns)
prediction = model.predict(new_street)[0]
probability = model.predict_proba(new_street)[0][1]

status = "FLOOD RISK (Avoid Street)" if prediction == 1 else "PASSABLE (Safe to Cross)"
print(f"Prediction: {status}")
print(f"Estimated Flood Probability: {probability * 100:.1f}%")

Data sources and AI methods

BahaWatch AI depends on real-time rainfall data from national weather agencies like PAGASA, local elevation and drainage maps, live traffic speed data, and crowdsourced street photos. While official weather and traffic feeds are reliable, user-submitted images require automated filtering to ensure they are clear, current, and accurate. To process this data, the project uses computer vision (Convolutional Neural Networks) to estimate water depth from uploaded photos, alongside time-series machine learning models to forecast which specific intersections will flood next based on rainfall rates, tide levels, and historical flood logs.

Challenges

BahaWatch AI predicts floods, but it cannot prevent them. It does not solve underlying urban issues like clogged drainage systems, poor waste management, or a lack of safe alternate routes when an entire district goes underwater. Additionally, the AI's predictions may be less accurate in lower-income neighborhoods with weak mobile internet coverage, where fewer residents can upload real-time photos during a storm.

What next?

In the future, BahaWatch AI could partner with popular navigation apps like Waze or Google Maps to automatically reroute drivers around predicted flood zones. Over time, the historical data collected by the app could also be shared with urban planners to run simulations and test how new drainage projects or green spaces would reduce future city flooding.


Acknowledgments

This project is inspired by Philippine civic tech initiatives like Project NOAH (Nationwide Operational Assessment of Hazards) and open-source flood mapping research. A working prototype would credit and rely on open datasets from OpenStreetMap, publicly available meteorological APIs, and open-source Python libraries such as Scikit-learn and TensorFlow.
