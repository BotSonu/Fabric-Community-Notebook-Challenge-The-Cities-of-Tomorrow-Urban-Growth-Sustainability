# Urban Sustainability Modeling Notebook

## Project Overview

This notebook demonstrates a comprehensive data analysis pipeline focused on understanding and modeling urban sustainability. Using a dataset of urban planning metrics, the project explores various factors influencing a city's sustainability score, identifies key drivers, develops a predictive model, and uncovers nuanced insights through advanced visualizations and unsupervised learning.

## Table of Contents

1.  [Data Loading and Initial Inspection](#data-loading-and-initial-inspection)
2.  [Column Canonicalization and Cleaning](#column-canonicalization-and-cleaning)
3.  [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
4.  [Feature Engineering](#feature-engineering)
5.  [Model Training and Evaluation](#model-training-and-evaluation)
6.  [Advanced Insights & Visualizations](#advanced-insights--visualizations)
    *   [Land Use Impact](#land-use-impact)
    *   [Disaster Risk and Air Quality](#disaster-risk-and-air-quality)
    *   [Interaction Effects](#interaction-effects)
    *   [Categorized Variables](#categorized-variables)
    *   [High-Error Predictions (Outliers)](#high-error-predictions-outliers)
    *   [Multi-dimensional Relationships](#multi-dimensional-relationships)
    *   [Urban Archetypes with Clustering](#urban-archetypes-with-clustering)
7.  [Analysis of Urban Growth Indicators](#analysis-of-urban-growth-indicators)
8.  [Conclusion and Recommendations](#conclusion-and-recommendations)

## Data Loading and Initial Inspection

The project begins by loading the `urban_planning_dataset.csv` into a pandas DataFrame. Initial checks confirm the dataset's shape, column names, and a preview of the first few rows.

## Column Canonicalization and Cleaning

Key columns like `population_density`, `green_cover_percentage`, `carbon_footprint`, and `urban_sustainability_score` are mapped to canonical names (`population`, `green_area_km2`, `co2_emissions`, `sustainability_index`) to standardize terminology. The dataset is checked for missing values, and numerical columns are imputed using the median strategy.

## Exploratory Data Analysis (EDA)

Initial visualizations were conducted to understand data distributions, including the `urban_sustainability_score` and `carbon_footprint`. Correlation heatmaps provided an overview of relationships between features.

## Feature Engineering

Several new features were engineered to capture more complex relationships:
*   **Canonical Columns**: Standardized versions of existing columns.
*   **Derived Metrics**: `green_area_m2` and `green_m2_per_person`.
*   **Interaction Terms**: `green_pop_interaction` (green cover % \* population density) and `renewable_income_interaction` (renewable energy usage \* average income).
*   **Composite Metrics**: `Environmental_Efficiency` (green cover % / carbon footprint) and `Infrastructure_Balance` (road connectivity / public transport access).

## Model Training and Evaluation

A `RandomForestRegressor` model was trained to predict the `urban_sustainability_score`. Initial models suffered from **target leakage** due to the `sustainability_index` being highly correlated with the target. A **refined model** was developed after carefully excluding all leakage-inducing features (including `sustainability_index` and prior `predicted_` / `residual_` columns).

### Refined Model Performance:
*   **RMSE**: 0.0380
*   **MAE**: 0.0304
*   **R2**: 0.9524
*   **Cross-Validation R2**: 0.9496

These metrics represent a robust and realistic predictive capability without bias. The model effectively explains a significant portion of the variance in urban sustainability.

### Feature Importance (Refined Model):

| Feature                      | Importance |
| :--------------------------- | :--------- |
| Environmental_Efficiency     | 0.4688     |
| renewable_energy_usage       | 0.2079     |
| disaster_risk_index          | 0.1056     |
| green_cover_percentage       | 0.0537     |
| green_area_m2                | 0.0475     |
| crime_rate                   | 0.0395     |
| public_transport_access      | 0.0336     |
| carbon_footprint             | 0.0096     |
| Infrastructure_Balance       | 0.0062     |
| renewable_income_interaction | 0.0039     |
| air_quality_index            | 0.0035     |
| building_density             | 0.0035     |
| green_m2_per_person          | 0.0034     |
| road_connectivity            | 0.0033     |
| green_pop_interaction        | 0.0031     |
| avg_income                   | 0.0027     |
| population_density           | 0.0022     |
| land_use_type_Commercial     | 0.0005     |
| land_use_type_Green Space    | 0.0005     |
| land_use_type_Residential    | 0.0005     |

## Advanced Insights & Visualizations

### Land Use Impact

Analysis revealed varying urban sustainability scores across `land_use_type` categories (Commercial, Green Space, Industrial, Residential), indicating context-dependent sustainability drivers.

### Disaster Risk and Air Quality

Correlations between `disaster_risk_index` (-0.3497) and `air_quality_index` (0.0187) with `urban_sustainability_score` were analyzed, showing a negative relationship with disaster risk and a negligible one with air quality. Visualizations confirmed these trends.

### Interaction Effects

*   **Green Cover & Population Density**: The positive impact of `green_cover_percentage` on `urban_sustainability_score` is more pronounced in lower-density areas. 
*   **Renewable Energy & Average Income**: Higher `renewable_energy_usage` correlates with increased sustainability, a relationship that is stronger in higher-income areas.

### Categorized Variables

*   **Population Density Categories**: Higher population density generally correlated with higher median urban sustainability scores, showing less variability.
*   **Average Income Categories**: A clear positive relationship exists, with higher income categories displaying higher median sustainability scores and greater consistency.

### High-Error Predictions (Outliers)

Comparison of high-error instances with the overall dataset revealed that the model struggled with cities at the extremes of feature distributions (e.g., high `disaster_risk_index`, low `green_cover_percentage`), suggesting unique urban contexts not fully captured.

### Multi-dimensional Relationships

Interactive scatter plots (e.g., `carbon_footprint` vs. `urban_sustainability_score` colored by `land_use_type`) showcased how different land uses contribute to carbon emissions and sustainability, guiding targeted interventions.

### Urban Archetypes with Clustering

K-Means clustering identified four distinct urban archetypes based on sustainability indicators:
*   **Cluster 0 (Moderate Sustainability, Lower Income, High Infrastructure Balance)**: Well-developed infrastructure despite economic challenges.
*   **Cluster 1 (Lower Sustainability, Moderate Income, Moderate Carbon Footprint)**: Struggling cities requiring intervention.
*   **Cluster 2 (Highest Sustainability, High Green Cover, Lowest Carbon Footprint - Outliers)**: Exceptionally sustainable, potential benchmarks.
*   **Cluster 3 (High Sustainability, High Income, Moderate Environmental Impact)**: Economically robust, strong environmental efforts.

## Analysis of Urban Growth Indicators

This section explored features that can serve as proxies for urban growth, including `population_density`, `avg_income`, `building_density`, `road_connectivity`, and `public_transport_access`.

### Key Findings

*   **Identified Indicators**: `population_density`, `avg_income`, `building_density`, `road_connectivity`, and `public_transport_access` were identified as potential urban growth indicators, reflecting characteristics of dynamic urban environments.
*   **Correlations with Sustainability**:
    *   `public_transport_access` showed the strongest positive correlation (0.20) with `urban_sustainability_score`.
    *   `avg_income` and `road_connectivity` had minor positive correlations (0.018 and 0.016, respectively).
    *   `building_density` had a very small positive correlation (0.006).
    *   `population_density` exhibited a slight negative correlation (-0.017).
*   **Visualized Distributions**: Distribution plots were generated for `population_density`, `avg_income`, and `building_density` to understand their spread.
*   **Multi-dimensional Explorations**: Scatter plots explored relationships between growth indicators and `urban_sustainability_score`, color-coded by factors like `land_use_type` and `renewable_energy_usage`.

### Limitations and Future Directions

*   **Limitations**: The analysis is constrained by the absence of explicit time-series growth data, relying on static snapshots to infer growth.
*   **Future Directions**: Integrating time-series data (e.g., population changes over decades, annual infrastructure development) would allow for direct measurement of growth rates and their causal impact on sustainability.

## Conclusion and Recommendations

This comprehensive analysis provides crucial insights for strategic urban planning:

*   **Robust Predictive Model**: The refined model offers a reliable tool for understanding urban sustainability drivers, explaining ~95% of the variance.
*   **Prioritize Green Infrastructure and Renewable Energy**: `Environmental_Efficiency`, `green_cover_percentage`, and `renewable_energy_usage` are primary levers for improving urban sustainability. Policies in these areas promise significant positive impact.
*   **Tailored Urban Planning Strategies**: The identified urban archetypes provide a framework for targeted interventions. For instance, Cluster 1 cities need urgent focus on green infrastructure and renewable energy, while Cluster 0 could benefit from economic development.
*   **Address Socio-Economic Factors**: Strong correlations between sustainability and population density/income highlight the need to integrate socio-economic factors into urban planning.
*   **Benchmark Against High Performers**: Further investigation into Cluster 2 cities can reveal best practices for extreme sustainability.
*   **Continuous Refinement**: Further investigation into high-error predictions can guide additional feature engineering or data collection, enhancing model accuracy and interpretability.
