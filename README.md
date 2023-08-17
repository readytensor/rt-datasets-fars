# Fatality Analysis Reporting System (FARS) Dataset for Multiclass Classification

## Overview

The **Fatality Analysis Reporting System (FARS)** dataset provides comprehensive data on fatal injuries stemming from motor vehicle traffic crashes. The dataset is a culmination of a census, capturing fatal traffic accidents across all 50 U.S. states, the District of Columbia, and Puerto Rico. The information is meticulously collected from state data files annually and then coded by analysts at the National Highway Traffic Safety Administration (NHTSA).

This dataset serves as an invaluable tool in:

- Analyzing the underlying causes and circumstances of motor vehicle traffic crashes.
- Formulating effective crash countermeasures.
- Procuring detailed insights on fatal crashes to guide the development, implementation, and evaluation of federal and state highway safety programs.

## Data Source

You can find more information about **FARS** at [https://www.nhtsa.gov/crash-data-systems/fatality-analysis-reporting-system](https://www.nhtsa.gov/crash-data-systems/fatality-analysis-reporting-system).

The dataset is extracted from here:  
https://cdan.dot.gov/query

## Objective

The core aim of this dataset is to enable predictions on the predominant factors influencing a crash, specifically: drunk-driving, speeding, or other determinants. Even though multiple factors might play a role in a single crash, our labeling approach assigns a unique tag to each instance. Consequently, this dataset serves as a resource for multiclass classification tasks.

## Data Preparation and Description

This section delves into the process of gathering, refining, and defining our dataset, from raw data acquisition at the NHTSA site to the finalized, purpose-built dataset.

### Dataset Composition

This dataset amalgamates information from three distinct sources:

1. **Crash Data**: This encapsulates features pertinent to individual crash incidents. It includes details like the date and time of the crash, the manner of collision, the location of the accident (including region, state, latitude, longitude, etc.), and the type of roadway on which the accident occurred.

2. **Vehicle Data**: This section offers insights into the vehicles implicated in the crash. It delves into aspects like the vehicle type, its make, and the model year.

3. **Driver Data**: This subset presents features specifically linked to the drivers embroiled in these mishaps.

### Time Frame and Data Selection

The dataset covers data from 2016 to 2018 and includes crashes with the following characteristics:

- Crashes that led to fatalities.
- Crashes involving only a single vehicle.
- The driver's age being at least 13 years.

The rationale behind these criteria is as follows:

- **Single-Vehicle Crashes**: Restricting our focus to single-vehicle crashes helps to accentuate the behaviors and decisions of one driver, without the confounding influence of other involved drivers.
- **Driver Age**: While the legal driving age in most regions is 16 years or older, we included younger drivers down to the age of 13 to capture instances of underage driving that resulted in fatal outcomes. It's worth noting that the original dataset contained even younger drivers, some as young as 5 years old. However, we set a lower age limit to ensure the data represents more plausible real-world scenarios, while still acknowledging the reality of underage driving incidents.

This dataset aims to provide a comprehensive view of fatal crashes, emphasizing the factors that might lead to such unfortunate events.

### Target Definition

The main objective of the dataset is to categorize driving incidents based on their primary influencing factor. In the original data, it's notable that multiple factors could influence a single crash. For instance, a driver under the influence of alcohol might also be speeding. However, for the purpose of this dataset and to maintain a clear categorization, we chose to apply a sequential and discrete labeling system for each vehicular incident.

The process to generate this label is as follows:

1. Crashes involving alcohol consumption, where the driver was found to have a Blood Alcohol Concentration above the legal limit, are labeled as `drunk driver involved`. This category takes precedence even if speeding or other factors influenced the crash.
2. If the crash was not influenced by alcohol but driver was found to be speeding, it is labeled as `speeding_driver_involved`.
3. Incidents that do not fall into the above categories, where neither alcohol nor speeding was involved, are labeled as 'other'.

This systematic categorization aims to provide a clear distinction between the main factors leading to fatal vehicular incidents.

### Train and Test Split

To facilitate the model evaluation and ensure a robust understanding of the model's performance, the dataset is divided into training and testing subsets performed using random sampling. Specifically:

- **Training Set**: Comprising 80% of the total samples, this subset will primarily be used for training machine learning models.

- **Test Set**: The remaining 20% of the samples are reserved for testing. Notably, the targets for this test set have been withheld to ensure unbiased evaluation. This approach ensures that the model's accuracy, precision, recall, and other performance metrics are assessed based on its predictions for unseen data.

This random 80/20 split ensures that models are not overfitting to the data, and it offers a reliable evaluation metric for gauging their real-world performance.

## Summary Statistics

Below are key statistics summarizing the structure and composition of the dataset:

- **Total Records**: 56,608

  - **Training Set**: 45,286 records
  - **Test Set**: 11,322 records

- **Columns Overview**:

  - Total: 39 columns
    - **ID Column**: 1 (`u_id`)
    - **Target Column**: 1 (`driver_factor`)
    - **Feature Columns**: 37
      - **Categorical Features**: 24
      - **Numerical Features**: 13

- **Missing Data**: Some features contain missing values.

- **Target Variable (`driver_factor`)**:
  - **Total Classes**: 3
  - **Class Distribution**:
    - `speeding_driver_involved`: 9,420 (16.6%)
    - `drunk_driver_involved`: 14,829 (26.2%)
    - `other`: 32,359 (57.2%)

## Data Dictionary

### **Id Field**

- **`u_id`**:
  - _Description_: A uniquely generated identifier for each crash incident.
  - _Type_: Identifier
  - _Note_: This field is custom generated and not part of the original dataset.

### **Target Field**

- **`driver_factor`**:
  - _Description_: Categorizes the primary factor responsible for the crash.
  - _Type_: Categorical
  - _Values_:
    - `speeding_driver_involved`: Denotes speeding as the main factor.
    - `drunk_driver_involved`: Indicates alcohol influence.
    - `other`: Neither speeding nor alcohol was the primary influence.

### Features

The following is a list of features in the dataset, grouped by category.

> **Note**: For detailed information on data types (numeric or categorical) and categories for categorical features, please refer to the `fars_schema.json` file located in the `./data/` folder. The schema also specifies which features are nullable.

#### Crash-related

- **`fatals`**: Number of fatalities in the event
- **`a_ct`**: Type of crash: single, two-vehicle, more than 2 vehicle
- **`a_ped_f`**: Whether pedestrian fatality involved in crash
- **`a_pedal_f`**: Whether pedalcyclist fatality involved in crash
- **`a_roll`**: Whether vehicle rollover involved in crash
- **`a_hr`**: Whether hit-and-run involved in crash
- **`a_polpur`**: Whether police pursuit involved in crash

#### Time of crash related

- **`month`**: Month of year when crash occurred
- **`day`**: Day of month when crash occurred
- **`day_week`**: Day of the week when crash occurred
- **`hour`**: Hour of the day when crash occurred
- **`minute`**: Minute in the hour when crash occurred
- **`a_dow_type`**: Day of week type: weekday (M-F) or weekend (Sat-Sun)
- **`a_tod_type`**: Time of day time: daytime (6 am to 6 pm), nighttime (6 pm to 6 am)

#### Location-related

- **`state`**: State in which crash occurred
- **`a_region`**: Region (made up of states) where crash occurred
- **`a_ru`**: Rural or urban
- **`a_inter`**: Whether crash occurred on Interstate highway
- **`a_intsec`**: Whether crash occurred at an intersection or not
- **`a_roadfc`**: Type of road (interstate, local, etc.)
- **`a_junc`**: Identifies if crash occurred in or proximity to junction or interchange area of two or more roadways
- **`a_relrd`**: Identifies area of roadway where crash occurred (on, off, shoulder, median, etc.)

#### People-related

- **`age`**: Age of driver
- **`permvit`**: Number of persons in motor vehicles in-transport
- **`pernotmvit`**: Number of persons not in motor vehicles in-transport
- **`a_ped`**: Whether crash involved a pedestrian
- **`numoccs`**: Number of motor vehicle occupants

#### Vehicle-related

- **`ve_forms`**: Number of vehicle forms submitted for mv in transport
- **`ve_total`**: Number of vehicle forms submitted
- **`mod_year`**: Vehicle model year
- **`a_body`**: Vehicle body type (automobile, light trucks, mediu/high trucks, buses, etc.)
- **`owner`**: Type of registered owner of vehicle in crash
- **`deaths`**: Number of fatalities in vehicle
- **`impact1`**: Areas of impact - initial contact point
- **`deformed`**: Extent of damage to vehicle

#### Environment-related

- **`weather`**: Prevailing atmospheric conditions that existed at the time of the crash
- **`lgt_cond`**: Type/level of light that existed at the time of the crash

## Project Structure

- `data/`: Contains the dataset files.
  - `fars_schema.json`: Contains the schema for the dataset. Schema file follows the Ready Tensor specifications for schema files for multiclass classification tasks. See details of the schema specifications here: [https://docs.readytensor.ai/for-contributors/model-requirements/multiclass-classification#data-schema-and-input--output-formats](https://docs.readytensor.ai/for-contributors/model-requirements/multiclass-classification#data-schema-and-input--output-formats)
  - `fars_train.csv`: Contains the training data in CSV format. The file contains the target column.
  - `fars_test.csv`: Contains the test data in CSV format. The file does not contain the target column.
  - `Fatality Analysis Reporting System Analytical User’s Manual, 1975-2021.pdf`: This is the original documentation for the dataset sourced directly from FARS. This manual contains more information on each of the features in the dataset.
- `.gitignore`: Lists files and directories that are not tracked by Git.
- `README.md`: Provides an overview and detailed description of the project, including how to use and understand the dataset.

## License

This project, including the code, analyses, and documentation, is licensed under the MIT License. However, please note that the dataset is sourced from the **FARS** and is in the public domain. You can find detailed licensing information in our [LICENSE.md](LICENSE.md) file.

## Contact Information

For questions, clarifications, or collaboration inquiries related to this project, please reach out to:

- **Name**: Ready Tensor, Inc.
- **Email**: contact@readytensor.com
- **Website**: [https://www.readytensor.ai](https://www.readytensor.ai)
