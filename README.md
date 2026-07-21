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

## 🚀 Key Findings & Results
 
- **Two-stage clustering was necessary to satisfy business constraints.** A single flat K-Means pass with a elbow value (**k=8**) produced technically sound clusters, but they ranged from ~165 to ~1,286 songs each — far too large to work as a playlist. Re-clustering each broad group individually produced **28 final playlists sized ~113–268 songs**, matching Moosic's usable playlist size range.
- **The clusters read as musically coherent genres, not noise.** Playlist 12 (drawn from the "Classical" branch) surfaces Beethoven, Chopin, Dvořák, and Barber; Playlist 9 is dominated by doom/death metal tracks. Distinct, high-signature genres like classical and metal are separated cleanly by audio features alone.
- **Audio features only partially substitute for human musical judgment.** They work well for genres with strong, consistent acoustic signatures (metal, classical) but blur together for genres distinguished mainly by language, lyrical content, or cultural context.
- **The mathematically "best" k and the business-usable k are different numbers.** The elbow method suggests a broad structure around k=8, but Moosic needs on the order of 20–30 playlists to hit its target playlist size — hence the two-stage hierarchical design.
- **28 playlists were mapped to 10 dominant-genre groups** for the team's review (Pop, Rock, Metal, EDM, Classical, Jazz, Hip-Hop/Rap/R&B, Latin, Indie Pop, Ambient/Acoustic) — see the picture below in Visualizations.

## 🛠️ Technologies Used
 
- **Programming:** Python
- **Libraries:** pandas,scikit-learn, seaborn.
- **Machine Learning:** K-Means clustering (two-stage / hierarchical application), `RobustScaler`, elbow method (inertia) and silhouette score for choosing k
- **Environment:** Google Colab

## 📁 Project Structure
 
```
moosic-playlist-clustering/
├── README.md                          # This file
├── data/
│   ├── spotify_5000_songs.csv         # Raw dataset (Spotify audio features, ~5,235 songs)
│   └── final_playlist_sizes.csv       # Recomputed size of each of the 28 final playlists
├── notebooks/
│   └── moosic_analysis.ipynb          # Main analysis notebook (cleaning, scaling, two-stage K-Means, playlist labeling)
└── images/
    ├── 00_hierarchy_overview.png      # Full two-stage clustering tree: all 8 broad clusters -> all 28 final playlists
    ├── 01_elbow_full_dataset.png      # Stage 1 elbow plot (full dataset, k=2-19)
    └── 02_playlist12_sample.png       # Sample of 10 tracks from Playlist 12 (Classical / Solo Piano & Orchestral)
```
 
## 📈 Visualisations
 
![Two-stage clustering hierarchy](images/00_hierarchy_overview.png)
*The full pipeline in one picture: Stage 1 splits all 5,160 songs into 8 broad K-Means clusters; Stage 2 re-clusters each broad group individually (or leaves it untouched when it's already playlist-sized) to produce the final 28 playlists, each labeled by its dominant audio-feature profile.*
 
![Stage 1 elbow plot](images/01_elbow_full_dataset.png)
*Inertia vs. number of clusters on the full, robust-scaled dataset. The curve bends around k=8 and  k=8 was chosen as the Stage 1 broad grouping to balance model simplicity with capturing distinct musical regions.*
 
![Playlist 12 sample](images/02_playlist12_sample.png)
*A random sample of 10 tracks from Playlist 12 ("Classical / Solo Piano & Orchestral," 140 songs). Beethoven, Chopin, Dvořák, and Barber all land in the same cluster, and every sampled track shares the same acoustic signature: low energy, low danceability, very high acousticness — concrete evidence the prototype produces musically cohesive playlists.*
 
