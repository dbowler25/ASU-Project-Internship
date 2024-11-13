# Final DataFrame

import pandas as pd

### Load the admissions, patients, diagnoses, procedures, and icustays data from CSV files
admissions_df = pd.read_csv('/Users/briannaalmasan/Desktop/ASUProject/Data/admissions.csv')
patients_df = pd.read_csv('/Users/briannaalmasan/Desktop/ASUProject/Data/patients.csv')
diagnoses_df = pd.read_csv('/Users/briannaalmasan/Desktop/ASUProject/Data/diagnoses_icd.csv')
procedures_df = pd.read_csv('/Users/briannaalmasan/Desktop/ASUProject/Data/procedures_icd.csv')
icustays_df = pd.read_csv('/Users/briannaalmasan/Desktop/ASUProject/Data/icustays.csv')

### Merge admissions and patients data
combined_df = pd.merge(admissions_df, patients_df, on='subject_id')

### Calculate the total count of diagnoses for each admission
diagnoses_count = diagnoses_df.groupby('hadm_id').size().reset_index(name='diagnosis_count')

### Merge the diagnosis counts with the combined DataFrame
combined_df_with_diagnoses = pd.merge(combined_df, diagnoses_count, on='hadm_id', how='left')

### Calculate the total count of procedures for each admission
procedures_count = procedures_df.groupby('hadm_id').size().reset_index(name='procedure_count')

### Merge the procedure counts with the DataFrame from Task 2
final_df = pd.merge(combined_df_with_diagnoses, procedures_count, on='hadm_id', how='left')

### Display the first few rows of the final DataFrame
print("Final DataFrame with Diagnosis and Procedure Counts:")
print(final_df.head())

### Create a flag column for ICU stay
### Assuming there is an 'admission_id' in the icustays data
icustays_flag = icustays_df[['hadm_id']].drop_duplicates()
icustays_flag['icu_stay'] = 1

### Merge the ICU stay flag with the final DataFrame
final_df_with_icu_flag = pd.merge(final_df, icustays_flag, on='hadm_id', how='left')

### Fill NaN values with 0 (indicating no ICU stay)
final_df_with_icu_flag['icu_stay'] = final_df_with_icu_flag['icu_stay'].fillna(0)

### Ensure the 'icu_stay' column is of integer type
final_df_with_icu_flag['icu_stay'] = final_df_with_icu_flag['icu_stay'].astype(int)

### Display the first few rows of the final DataFrame with ICU flag
print("Final DataFrame with ICU Stay Flag:")
print(final_df_with_icu_flag.head())

### Optionally, save the final DataFrame to a new CSV file
final_df_with_icu_flag.to_csv('/Users/briannaalmasan/Desktop/ASUProject/Data/final_combined_admissions_patients_diagnoses_procedures_icu.csv', index=False)
