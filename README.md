import pandas as pd

df = pd.read_csv("netflix_movies_tvshows_raw.csv")

print(df.info())
print(df.head())
df = df.drop_duplicates()
# Fill missing titles or descriptions with "Unknown"
df['title'] = df['title'].fillna("Unknown Title")
df['description'] = df['description'].fillna("No description available")

# Drop rows where director or cast is missing (or use fillna if preferred)
df = df.dropna(subset=['director', 'cast'])
df['country'] = df['country'].str.strip().str.lower()
df['country'] = df['country'].replace({
    'usa': 'united states',
    'uk': 'united kingdom'
})
df['date_added'] = pd.to_datetime(df['date_added'], errors='coerce')
# Separate TV Shows (Seasons) and Movies (Minutes)
df['duration_clean'] = df['duration'].str.extract(r'(\d+)').astype(float)
df['duration_type'] = df['duration'].apply(lambda x: 'Season' if 'Season' in x else 'Minutes')
df['rating'] = df['rating'].replace({
    'PG13': 'PG-13'
})
