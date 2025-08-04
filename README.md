import pandas as pd

# Load the dataset
df = pd.read_csv("netflix_titles.csv")

# View first few rows
print(df.head())

# 1. Check for missing values
print(df.isnull().sum())

# Fill missing 'rating' with most frequent value
df['rating'].fillna(df['rating'].mode()[0], inplace=True)

# Fill missing 'country' with "Unknown"
df['country'].fillna("Unknown", inplace=True)

# Drop rows with missing 'date_added'
df.dropna(subset=['date_added'], inplace=True)

# 2. Remove duplicates
df.drop_duplicates(inplace=True)

# 3. Standardize text: lowercase column 'type'
df['type'] = df['type'].str.lower()

# 4. Convert 'date_added' to datetime format
df['date_added'] = pd.to_datetime(df['date_added'])

# 5. Strip whitespace from column names
df.columns = df.columns.str.strip()

# 6. Final check
print(df.info())

# Save cleaned data
df.to_csv("netflix_cleaned.csv", index=False)
