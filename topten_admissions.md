# Top 10 for this file: admissions.csv

pip install pandas
import pandas as pd
import os

### Define the file path and expand the home directory
file_path = os.path.expanduser('~/Desktop/ASUProject/Data/admissions.csv')

### Check if the file exists
if not os.path.exists(file_path):
    print(f"File not found: {file_path}")
else:
    # Load the dataset into a DataFrame
    df = pd.read_csv(file_path)

    # Display the first 10 rows of the DataFrame
    print("First 10 rows of the DataFrame:")
    print(df.head(10))

    # Provide a summary of the DataFrame using the info() method
    print("\nSummary of the DataFrame:")
    df.info()
