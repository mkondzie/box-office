# Box Office Performance

This project analyzes movie revenue data, enriches it with movie details from the OMDb API, and presents a ranking dashboard based on various metrics. The notebook includes a data pipeline, including API data fetching with caching to manage API limits, data cleaning, creation of a star schema (fact and dimension tables), and visualization.

## Table of Contents
- [Setup and Installation](#setup-and-installation)
- [Data Files](#data-files)
- [OMDb API Key](#omdb-api-key)
- [Project Structure](#project-structure)
- [Data Pipeline Overview](#data-pipeline-overview)
- [Ranking Dashboard Metrics](#ranking-dashboard-metrics)
- [Usage](#usage)
- [Caching Mechanism](#caching-mechanism)
- [Cleanup Guidelines](#cleanup-guidelines)

## Setup and Installation

1.  **Google Colab**: This notebook is designed to run in Google Colab.
2.  **Mount Google Drive**: The notebook expects the `revenues_per_day.csv` file to be located in a specific path within your Google Drive. Ensure you mount your Google Drive at `/content/drive` within the Colab environment. The code includes a cell for this:
    ```python
    from google.colab import drive
    drive.mount('/content/drive')
    ```
3.  **Install Libraries**: The project uses `pandas`, `numpy`, `requests`, `python-dotenv`, `matplotlib`, and `seaborn`. These can be installed using pip if not already available in your Colab environment:
    ```bash
    !pip install pandas numpy requests python-dotenv matplotlib seaborn
    ```

## Data Files

*   **`revenues_per_day.csv`**: This is the primary input data file, containing daily movie revenue records.
    *   **Location**: It is expected to be found at `/content/drive/MyDrive/revenues_per_day.csv`. Please adjust the path in the notebook if your file is located elsewhere.

## OMDb API Key

To fetch movie details, an OMDb API key is required.

1.  **Obtain an API Key**:
    *   Visit the [OMDb API website](http://www.omdbapi.com/apikey.aspx).
    *   Register for a free API key.
2.  **Store the API Key in Google Colab Secrets**:
    *   In your Google Colab notebook, click on the key icon (Secrets) in the left sidebar.
    *   Add a new secret with the name `API_KEY` and paste your OMDb API key as its value.
    *   Ensure "Notebook access" is toggled ON for this secret.
    *   The notebook will then retrieve the API key using `userdata.get("API_KEY")`.
  
## Project Structure

The notebook follows a structured approach:

1.  **Data Loading**: Loads `revenues_per_day.csv` from Google Drive.
2.  **API Integration**: Fetches additional movie details from the OMDb API.
3.  **Caching**: Implements a caching mechanism to save fetched API data to a local JSON file (`/content/movies_omdb_full.json`) to prevent repeated API calls and manage daily limits.
4.  **Data Modeling**: Transforms the raw data into a star schema, comprising:
    *   **`fact_df`**: The fact table, containing daily revenue and theater counts, linked to dimension tables by foreign keys.
    *   **`movie_df`**: The movie dimension table, storing detailed movie information.
    *   **`date_df`**: The date dimension table, containing date-related attributes.
5.  **Data Cleaning**: Handles missing values and corrects data types.
6.  **Ranking Dashboard**: Aggregates data and visualizes top-ranked movies based on various performance indicators.

## Data Pipeline Overview

The notebook executes a consolidated data pipeline to ensure reproducibility and clarity:

1.  Loads `revenues_per_day.csv` into `df`.
2.  Initializes `movie_df` from `movie_records` (a variable populated from an initial API call, demonstrating the caching approach).
3.  Saves `movie_df` to `/content/movies_omdb_full.json` for persistent caching.
4.  Processes `df` to create `date_df` with unique dates and time attributes.
5.  Assigns `movie_id` to `movie_df`.
6.  Filters `df` to include only movies for which OMDb data was retrieved.
7.  Merges `df`, `movie_df`, and `date_df` to form `fact_df`.
8.  Cleans and converts data types in `fact_df`.
9.  Converts `imdb_rating` and `imdb_votes` in `movie_df` to numeric types.
10. Calculates aggregated metrics (`total_revenue`, `theaters`, `imdb_rating`, `imdb_votes`) for the ranking dashboard.

## Ranking Dashboard Metrics

The dashboard visualizes top 10 rankings based on:

*   **Total Revenue**: Sum of all daily revenues for a movie.
*   **Number of Theaters**: The number of theaters a movie was screened in.
*   **IMDb Rating**: Movie rating from IMDb.
*   **IMDb Votes**: Number of votes received on IMDb.


## Usage

To run this notebook:

1.  Upload `revenues_per_day.csv` to `/content/drive/MyDrive/`.
2.  Obtain an OMDb API key and store it in Colab secrets as `API_KEY`.
3.  Open the notebook in Google Colab.
4.  Execute all cells in order. The pipeline is designed to be run sequentially.

## Caching Mechanism

The notebook incorporates a caching mechanism to avoid repeatedly hitting the OMDb API. After the initial API calls (or when the `movie_records` variable is available from a previous run), the fetched movie data is saved to `/content/movies_omdb_full.json`. In subsequent runs or if the API limit is reached, the notebook can load movie details directly from this JSON cache, ensuring continued functionality without re-fetching.
  
      
