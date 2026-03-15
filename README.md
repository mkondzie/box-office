# Movie Revenue Analysis and Ranking Dashboard

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
  
      
  
      
