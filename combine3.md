# Combine 3 Datasets

import pandas as pd

### Load the admissions, patients, and diagnoses data from CSV files
admissions_df = pd.read_csv('/Users/briannaalmasan/Downloads/ASUProject/Data/admissions.csv')
patients_df = pd.read_csv('/Users/briannaalmasan/Downloads/ASUProject/Data/patients.csv')
diagnoses_df = pd.read_csv('/Users/briannaalmasan/Downloads/ASUProject/Data/diagnoses_icd.csv')

### Merge admissions and patients data
combined_df = pd.merge(admissions_df, patients_df, on='subject_id')

### Display the first few rows of the combined DataFrame
print("Combined DataFrame (Admissions + Patients):")
print(combined_df.head())

### Display the column names to understand the structure
print("Diagnoses columns:", diagnoses_df.columns)

### Calculate the total count of diagnoses for each admission
### Assuming there are 'admission_id' and 'diagnosis_id' columns in the diagnoses data
diagnoses_count = diagnoses_df.groupby('hadm_id').size().reset_index(name='diagnosis_count')

### Merge the diagnosis counts with the combined DataFrame
final_df = pd.merge(combined_df, diagnoses_count, on='hadm_id', how='left')

### Display the first few rows of the final DataFrame
print("Final DataFrame with Diagnosis Counts:")
print(final_df.head())

### Optionally, save the final DataFrame to a new CSV file
final_df.to_csv('/Users/briannaalmasan/Desktop/ASUProject/Data/final_combined_admissions_patients_diagnoses.csv', index=False)
