# Combine Datasets

import pandas as pd

### Load the admissions and patients data from CSV files
admissions_df = pd.read_csv('/Users/briannaalmasan/Desktop/ASUProject/Data/admissions.csv')
patients_df = pd.read_csv('/Users/briannaalmasan/Desktop/ASUProject/Data/patients.csv')

### Display the first few rows of each DataFrame to understand their structure
print(admissions_df.head())
print(patients_df.head())

### Merge the two DataFrames on the common key (assumed to be 'patient_id')
### Adjust the key based on the actual column name in your datasets
combined_df = pd.merge(admissions_df, patients_df, on='subject_id')

### Display the first few rows of the combined DataFrame
print(combined_df.head())

### Optionally, save the combined DataFrame to a new CSV file
### combined_df.to_csv('combined_admissions_patients.csv', index=False)

### Optionally, save the combined DataFrame to a new CSV file in the specified directory
combined_df.to_csv('/Users/briannaalmasan/Desktop/ASUProject/Data/combined_admissions_patients.csv', index=False)
