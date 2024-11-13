# Readmission within 30 Days

import pandas as pd

### Load the admissions data if not already loaded
admissions_df = pd.read_csv('/Users/briannaalmasan/Desktop/ASUProject/Data/admissions.csv')

### Ensure datetime columns are in datetime format
admissions_df['admittime'] = pd.to_datetime(admissions_df['admittime'])

### Step 2: Sort by subject_id and admittime to compare consecutive admissions
admissions_df = admissions_df.sort_values(by=['subject_id', 'admittime'])

### Step 3: Create a new column for the readmission flag
admissions_df['readmitted_within_30_days'] = False

### Step 4: Iterate through the DataFrame to flag readmissions within 30 days
for i in range(1, len(admissions_df)):
    current_patient = admissions_df.iloc[i]
    previous_patient = admissions_df.iloc[i - 1]

    ### Check if the current and previous rows are for the same patient
    if current_patient['subject_id'] == previous_patient['subject_id']:
        ### Calculate the time difference between the current and previous admission
        time_diff = current_patient['admittime'] - previous_patient['admittime']
       
        ### Check if the previous admission was within 30 days of the current one
        if pd.Timedelta(days=0) < time_diff <= pd.Timedelta(days=30):
            admissions_df.at[i, 'readmitted_within_30_days'] = True

### Step 5: Inspect the DataFrame to check the new column
print(admissions_df[['subject_id', 'admittime', 'readmitted_within_30_days']].head(10))

### Step 6: Optionally, save the updated DataFrame to a CSV file
admissions_df.to_csv('/Users/briannaalmasan/Desktop/ASUProject/Data/admissions_with_readmission_flags.csv', index=False)
