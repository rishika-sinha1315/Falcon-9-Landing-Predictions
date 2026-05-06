# 🚀 SPACEX FALCON9 LANDING PREDICTIONS

## Project Overview
   This project predicts whether the Falcon9 first stage booster will successfully land after a launch. SpaceX advertises Falcon9 rocket launches at approximately $62million,compared to competitors who charge upward of $165 million per launches.
   A large part of the savings comes from reusing the first stage. By predicting landing success, we can estimate the cost of a launch and gain competitive intelligence.

## Table of Contents
- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Project Pipeline](#project-pipeline)
- [Technologies Used](#technologies-used)
- [Modules](#modules)
- [Results](#results)
- [How to Run](#how-to-run)


## Dataset

<details>
<summary>Click to view datasets used</summary>

| Dataset | Description |
|---------|-------------|
| dataset_part_1.csv | Raw launch data — orbits, outcomes, launch sites |
| dataset_part_2.csv | Engineered features — payload mass, grid fins, reuse counts |
| dataset_part_3.csv | One-hot encoded feature matrix (generated locally) |
| Spacex.csv | Full mission data loaded into SQL database |
| spacex_launch_geo.csv | Geospatial data for launch site mapping |

</details>


## Project Pipeline

  Raw Data → SQL & EDA Analysis → Feature Engineering → Visualization → ML Modeling → Evaluation

## Technologies Used
- Python 
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Folium
- Scikit-learn
- SQLite


## Modules

### 1. Data Collection & Wrangling
- Loaded Falcon 9 launch data from dataset_part_1.csv
- Checked missing values using isnull().sum()/len(df)*100
- Inspected data types using df.dtypes
- Counted value distributions for LaunchSite, Orbit and Outcome columns
- Identified bad landing outcomes and created binary Class column
  — 1 for successful landing, 0 for failed landing

### 2. SQL Analysis
- Connected SpaceX.csv to a SQLite database (my_data1.db)
- Queried unique launch sites and launches starting with CCA
- Calculated total payload mass for NASA (CRS) customer
- Found average payload mass for Booster Version F9 v1.1
- Retrieved first successful ground pad landing date
- Found booster versions with drone ship success and payload between 4000-6000 kg
- Counted all mission outcomes grouped by type
- Retrieved landing outcomes between 2010-06-04 and 2017-03-20

### 3. Exploratory Data Analysis (EDA)
- Loaded dataset_part_2.csv for visual analysis
- Plotted Flight Number vs Payload Mass colored by Class
- Plotted Flight Number vs Launch Site colored by Class
- Plotted Payload Mass vs Launch Site colored by Class
- Visualized success rate per Orbit type using a bar chart
- Plotted Flight Number vs Orbit and Payload Mass vs Orbit
- Extracted year from Date column and plotted yearly landing success rate

### 4. Interactive Visual Analytics
- Loaded spacex_launch_geo.csv for map plotting
- Built a Folium map centered on NASA Johnson Space Center
- Plotted all launches — green star for success, red X for failure
- Launch sites covered: CCAFS LC-40, CCAFS SLC-40, KSC LC-39A, VAFB SLC-4E
- Created a Plotly bar chart showing Success vs Failure count by Launch Site
- Created a Plotly pie chart showing overall launch success rate

### 5. Machine Learning Prediction
- Loaded dataset_part_2.csv (features) and dataset_part_3.csv (one-hot encoded)
- Split data 80% train and 20% test with random_state=2
- Scaled features using StandardScaler
- Trained and tuned 4 models using GridSearchCV with 10-fold cross validation:
  - Logistic Regression — tuned C, penalty, solver
  - SVM — tuned kernel, C, gamma
  - Decision Tree — tuned max_depth, criterion
  - KNN — tuned n_neighbors, algorithm, p
- Evaluated all models using accuracy score and confusion matrix


## Results

All 4 models were evaluated on the test set (20% of data).

| Model | Accuracy |
|---|---|
| Logistic Regression | 83.33% |
| SVM | 83.33% |
| Decision Tree | 77.78% |
| KNN | 83.33% |

The confusion matrix labels:
- land — Booster successfully landed
- did not land — Booster failed to land

Logistic Regression, SVM and KNN all achieved the highest
accuracy of 83.33%. Decision Tree performed slightly lower
at 77.78%.


## How to Run

1. Install dependencies:

```
pip install pandas numpy seaborn matplotlib scikit-learn
pip install folium plotly sqlalchemy ipython-sql prettytable
```

2. Open the notebook:

```
jupyter notebook Falcon9__final__1_.ipynb
```

3. Run all cells in order.

Note: All datasets are automatically fetched from IBM's
public cloud storage — no manual download required.
