# Solar Energy Prediction and Energy Conservation Analysis Using PySpark and Big Data Systems

Poster project for the **15th Biennial Lake Symposium (Lake 2026)**, 18–20 November 2026, Chanakya University, Bengaluru.
Theme 23: Renewable Energy and Energy Conservation.

**Author:** Abhishek R., BCA, Chanakya University

## What this project is about (in simple words)

Solar power goes up and down with the sun and the weather. If we can predict it, energy can be planned and saved instead of wasted.

This project uses **PySpark** (a big-data tool) to:

1. **Predict solar power** at two real solar plants in India from weather readings.
2. **Map solar potential** across India: which cities have the strongest sunlight, and which swing most between seasons.
3. **Plan energy for ecosystem monitoring:** how big a solar panel a small lake-monitoring station would need in each city to stay powered through the year (an illustrative design study).

## Datasets

| Dataset | What it is | Link |
|---|---|---|
| Solar Power Generation Data (Kaggle, by anikannal) | 2 solar plants in India, 34 days, readings every 15 minutes (power and weather) | https://www.kaggle.com/datasets/anikannal/solar-power-generation-data |
| NASA POWER | Hourly sunlight (ALLSKY_SFC_SW_DWN, Wh/m²) and air temperature (T2M, °C) for 57 Indian cities, 2018–2023 | https://power.larc.nasa.gov/ (API docs: https://power.larc.nasa.gov/docs/services/api/temporal/hourly/) |

The data files are **not stored in this repository**. The notebooks download them when you run them.

## Part A: plant power prediction (Kaggle data)

- 136,472 inverter-level rows after joining power with weather.
- Features: hour, minute, ambient temperature, module temperature, irradiation, plant.
- Time-based split: train on 106,948 rows before 11 June 2020, test on the 29,524 later rows.

| Model | MAE (kW) | RMSE (kW) | R² |
|---|---|---|---|
| Linear Regression | 79.607 | 137.077 | 0.821 |
| Random Forest | 44.048 | 119.014 | 0.865 |
| Gradient Boosted Trees | 41.255 | 115.554 | 0.873 |

Sunlight (irradiation) is by far the strongest driver of power. Plant 1 makes 62.0% and Plant 2 makes 54.4% of daily energy between 10:00 and 14:59.

## Part B: India-wide solar potential (NASA POWER data)

- 2,997,288 hourly records, 57 cities, 6 years, about 20 MB as Parquet. A full Spark query took about 1.6 seconds.
- Sunniest cities: Jaisalmer, Madurai, Jodhpur, Bikaner, Thiruvananthapuram.
- Biggest seasonal swing (need most storage or backup): Jammu, Amritsar, Srinagar, Chandigarh, Leh.

## Part C: energy planning for ecosystem monitoring (illustrative design study)

Assumptions: station load 50 W all day, battery 1,200 Wh, system efficiency 75%, target 99% of hours powered, flat-surface NASA sunlight (panel tilt not included).

[Add your Part C result lines here after running the notebook.]

This is a design example from city-level NASA estimates. It is **not** a measurement at a real lake, and the project does not measure water quality or ecosystem health.

## How to run

1. Open a notebook in **Google Colab** (File > Upload notebook).
2. Run the cells from top to bottom.
3. Part A downloads the Kaggle data automatically. Part B downloads the NASA data (about 10 to 30 minutes). Part C runs after Part B in the same session.

## Limitations

- Predictions use **measured** weather, so this is not yet a forecast from weather forecasts.
- NASA POWER values are city-level estimates, not readings at a particular site.
- Part C uses assumed load, battery and efficiency values.

## Future work

Add weather forecasts and cloud cover, try ensemble models, include more plants, and apply the pipeline to solar-powered lake monitoring stations.

## References

1. Kaggle, Solar Power Generation Data (anikannal).
2. NASA POWER Project, power.larc.nasa.gov.
3. Apache Spark and PySpark documentation, spark.apache.org.
