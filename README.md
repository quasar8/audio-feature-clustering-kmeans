# Moosic Playlist Generation: Audio-Feature-Based Song Clustering with K-Means
 
## 🎯 Project Overview
 
Moosic needed to automate playlist creation as manual curation couldn't scale with business growth. Using K-Means clustering on Spotify's audio features, we grouped ~5,160 songs into 28 cohesive playlists (116–268 songs each) via a two-stage hierarchical approach — revealing both a natural genre-level structure and its limits, since audio features capture acoustic character well but often miss cultural/genre identity.

## 📊 Dataset & Sources
 
- **Source:** Spotify API audio features (provided as `spotify_5000_songs.csv`)
- **Size:** 5,235 songs × 18 columns raw; after removing duplicates and non-modeling columns, 5,160 unique songs × 10 audio features used for clustering.
- **Key Features Used:** `danceability`, `energy`, `loudness`, `mode`, `speechiness`, `acousticness`, `instrumentalness`, `liveness`, `valence`, `tempo`
- **Features dropped:** `type`, `duration_ms`, `key`, `time_signature`, `id`, `html` — these are identifiers, metadata, or fields (like `id`/`html`) that carry no musical/audio signal for clustering. `name` and `artist` are kept as the index for readability, not as model inputs.
- **Data Limitations & Preprocessing:**
  - Column names contained trailing whitespace, cleaned with `.str.strip()`
  - Exact duplicate rows were removed (5,235 → 5,160 songs).
  - No missing values were present in the 10 modeling features.
  - Features were scaled using `RobustScaler` rather than `StandardScaler`, since several features (`loudness`, `speechiness`, `instrumentalness`, `liveness`) showed strong skew and outliers that would have distorted mean/std-based scaling
