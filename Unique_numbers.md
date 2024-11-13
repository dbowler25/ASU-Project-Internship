# Total Unique Admissions and Patients

pip install pandas
import pandas as pd
import os

### Replace 'path_to_your_file.csv' with the actual path to your CSV file
file_path = '~/Desktop/ASUProject/Data/admissions.csv'  # Adjust the path as needed

### Expand the path to ensure it works correctly
file_path = os.path.expanduser(file_path)

### Read the CSV file into a DataFrame
df = pd.read_csv(file_path)

### Check if 'subject_id' column exists
if 'subject_id' in df.columns:
    unique_patients = df['subject_id'].nunique()
else:
    unique_patients = None
    print("Warning: 'subject_id' column is not found in the dataset.")

### Check if 'hadm_id' column exists
if 'hadm_id' in df.columns:
    unique_admissions = df['hadm_id'].nunique()
else:
    unique_admissions = None
    print("Warning: 'hadm_id' column is not found in the dataset.")

### Print the results
print(f"Number of unique patients (subject_id): {unique_patients}")
print(f"Number of unique hospital admissions (hadm_id): {unique_admissions}")
