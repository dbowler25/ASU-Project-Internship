# Frequency Graph of Length of Stay

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

### Display the top 10 lengths of stay in hours
print(df['length_of_stay_hours'].head(25))

### Create a histogram to visualize the distribution of hospital stay lengths
plt.figure(figsize=(10, 6))
plt.hist(df['length_of_stay_hours'], bins=50, edgecolor='k', alpha=0.7)
plt.title('Distribution of Hospital Stay Lengths (in Hours)')
plt.xlabel('Length of Stay (Hours)')
plt.ylabel('Frequency')
plt.grid(True)
plt.show()
