# Moosic Playlist Generation: Audio-Feature-Based Song Clustering with K-Means
 
## 🎯 Project Overview
 
Moosic needed to automate playlist creation as manual curation couldn't scale with business growth. Using K-Means clustering on Spotify's audio features, we grouped ~5,160 songs into cohesive playlists (116–268 songs each) via a two-stage hierarchical approach — revealing both a natural genre-level structure and its limits, since audio features capture acoustic character well but often miss cultural/genre identity.

## 📊 Dataset & Sources
 
- **Source:** Spotify API audio features (provided as `spotify_5000_songs.csv`)
- **Size:** 5,235 songs originally → 5,160 after removing exact duplicates, 18 columns → 10 features used for clustering
- **Key Features Used:** `danceability`, `energy`, `loudness`, `mode`, `speechiness`, `acousticness`, `instrumentalness`, `liveness`, `valence`, `tempo`
- **Columns Dropped:** `id`, `html`, `type` (zero variance / non-numeric identifiers), `duration_ms` and `time_signature` (low variance, not meaningfully tied to mood), `key` (categorical, not well-suited to Euclidean distance without encoding)
- **Data Limitations & Preprocessing:**
  - Column names contained trailing whitespace, cleaned with `.str.strip()`
  - 148 exact duplicate rows removed; ~96 rows with identical (song, artist) but different audio features were kept, as these likely represent different recordings (live/remaster versions)
  - No missing values found in the final feature set
  - Features were scaled using `RobustScaler` (median/IQR-based) rather than `StandardScaler`, since several features (`loudness`, `speechiness`, `instrumentalness`, `liveness`) showed strong skew and outliers that would have distorted mean/std-based scaling
