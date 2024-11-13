# Calculate Length of Stay

import pandas as pd

### Load the admissions data if not already loaded
admissions_df = pd.read_csv('/Users/briannaalmasan/Desktop/ASUProject/Data/admissions.csv')

### Ensure datetime columns are in datetime format
admissions_df['admittime'] = pd.to_datetime(admissions_df['admittime'])
admissions_df['dischtime'] = pd.to_datetime(admissions_df['dischtime'])

### Calculate the length of stay in days
admissions_df['length_of_stay'] = (admissions_df['dischtime'] - admissions_df['admittime']).dt.days

### Inspect the new column
print(admissions_df[['admittime', 'dischtime', 'length_of_stay']].head())

### Optionally, save the updated DataFrame to a CSV file
admissions_df.to_csv('/Users/briannaalmasan/admissions_with_length_of_stay.csv', index=False)
