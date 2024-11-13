# Learn to Merge

import pandas as pd

### Load the required dataframes
diagnoses_icd_df = pd.read_csv('/Users/briannaalmasan/Downloads/ASUProject/Data/diagnoses_icd.csv')
icd_diagnoses_df = pd.read_csv('/Users/briannaalmasan/Downloads/ASUProject/Data/d_icd_diagnoses.csv')
admissions_df = pd.read_csv('/Users/briannaalmasan/Downloads/ASUProject/Data/admissions.csv')
patients_df = pd.read_csv('/Users/briannaalmasan/Downloads/ASUProject/Data/patients.csv')

### Step 1: Identify the primary diagnosis (where seq_num = 1)
primary_diagnoses = diagnoses_icd_df[diagnoses_icd_df['seq_num'] == 1]

### Step 2: Merge the primary diagnosis data with the icd_diagnoses_df to get the long_title
primary_diagnoses = primary_diagnoses.merge(icd_diagnoses_df, on=['icd_code', 'icd_version'], how='left')

### Step 3: Merge with the admissions data on 'hadm_id' and 'subject_id'
admissions_with_diagnoses = admissions_df.merge(primary_diagnoses[['subject_id', 'hadm_id', 'icd_code', 'icd_version', 'long_title']],
                                                on=['hadm_id', 'subject_id'], how='left')

### Step 4: Optionally, merge with the patients data if you need patient-specific details
final_df = admissions_with_diagnoses.merge(patients_df, on='subject_id', how='left')

### Step 5: Save the final DataFrame to a CSV file
final_df.to_csv('/Users/briannaalmasan/Desktop/ASUProject/Data/combined_admissions_patients_with_diagnoses.csv', index=False)

### You can print or inspect the final DataFrame
print(final_df.head())
