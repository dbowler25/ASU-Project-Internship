# List Prescriptions

import pandas as pd

### Load the prescriptions data
prescriptions_df = pd.read_csv('/Users/briannaalmasan/Downloads/ASUProject/Data/prescriptions-2.csv')

### Ensure datetime columns are in datetime format
prescriptions_df['starttime'] = pd.to_datetime(prescriptions_df['starttime'])
admissions_df['admittime'] = pd.to_datetime(admissions_df['admittime'])

### Initialize a DataFrame to store prescription counts
prescription_counts = []

### Step 3: Iterate over each admission and count prescriptions in the 1 month prior
for index, row in admissions_df.iterrows():
    subject_id = row['subject_id']
    hadm_id = row['hadm_id']
    admittime = row['admittime']
   
    ### Define the start of the 1 month prior period
    start_period = admittime - pd.Timedelta(days=30)
   
    ### Filter prescriptions within the 1 month prior to admission
    prescriptions_prior = prescriptions_df[
        (prescriptions_df['subject_id'] == subject_id) &
        (prescriptions_df['starttime'] >= start_period) &
        (prescriptions_df['starttime'] < admittime)
    ]
   
    ### Count the number of prescriptions
    prescription_count = prescriptions_prior.shape[0]
   
    ### Store the count with the corresponding hadm_id
    prescription_counts.append({'hadm_id': hadm_id, 'prescription_count': prescription_count})

### Convert the list of dictionaries to a DataFrame
prescription_counts_df = pd.DataFrame(prescription_counts)

### Step 5: Merge the prescription counts with the existing admissions DataFrame
final_df_with_prescriptions = final_df.merge(prescription_counts_df, on='hadm_id', how='left')

### Step 6: Save the final DataFrame to a CSV file
final_df_with_prescriptions.to_csv('/Users/briannaalmasan/Desktop/ASUProject/Data/combined_admissions_patients_with_prescriptions.csv', index=False)

### You can print or inspect the final DataFrame
print(final_df_with_prescriptions.head())
