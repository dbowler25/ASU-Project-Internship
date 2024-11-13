# Average Length of Stay per Admission Type

pip install pandas
import pandas as pd
import matplotlib.pyplot as plt

### File path to your data
file_path = '~/Desktop/ASUProject/Data/admissions.csv'

### Load the data into a DataFrame
df = pd.read_csv(file_path)

### Convert 'admittime' and 'dischtime' columns to datetime format, handling errors by setting invalid parsing as NaT
df['admittime'] = pd.to_datetime(df['admittime'], errors='coerce')
df['dischtime'] = pd.to_datetime(df['dischtime'], errors='coerce')

### Check for any rows with NaT values in 'admittime' or 'dischtime' and drop them
df = df.dropna(subset=['admittime', 'dischtime'])

### Calculate the length of stay in hours for each admission
df['length_of_stay_hours'] = (df['dischtime'] - df['admittime']).dt.total_seconds() / 3600

### Group by 'admission_type' and calculate the average length of stay for each type
average_length_of_stay = df.groupby('admission_type')['length_of_stay_hours'].mean()

### Display the results
print(average_length_of_stay)
