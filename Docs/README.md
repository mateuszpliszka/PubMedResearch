------------------------------ 
1st file
------------------------------
# PubMed Data Gathering and Sentiment Analysis Pipeline

This project provides a complete pipeline for gathering biomedical literature data from PubMed and performing sentiment analysis. It includes tools for:
- Querying and downloading PubMed articles via the PubMed API.
- Efficiently handling large datasets with progress tracking and checkpointing.
- Performing auto-labeling using a pre-trained model (BioMedBERT).

## Dataset

The labeled PubMed dataset used in this project is available on [Kaggle](https://kaggle.com/datasets/9896569a20d89768d95f0a4ce3fe2b459a5e36b6e2a56ef19d1b165c113be03b). It includes:
- **Raw PubMed abstracts** collected using the `0.API_Data_Gathering.ipynb` file.
- **Labeled abstracts** auto-labeled with sentiment using BioMedBERT.

## Project Structure

- **`0.API_Data_Gathering.ipynb`**: Collects PubMed abstracts.
- **`1.Data_Processing_and_Labelling.ipynb`**: Processes and labels abstracts.
- **`datasets/`**: Contains raw and labeled datasets.
- **`README.md`**: Overview and guide for this project.
- **`requirements.txt`**: Dependencies required to run the project.

## Getting Started

### Prerequisites
Install the dependencies:
```bash
pip install -r requirements.txt
#
```
# PROPER
# Project Title: PubMed Sentiment Analysis Pipeline

---

## Table of Contents

1. [Overview](#overview)  
2. [Getting Started](#getting-started)  
3. [Project Structure](#project-structure)  
4. [Usage Instructions](#usage-instructions)  
5. [Contributions](#contributions)  
6. [License](#license)  

---

## Overview

This repository provides a comprehensive pipeline to analyze PubMed abstracts for sentiment classification. The project includes data collection from PubMed using APIs, pre-processing, sentiment analysis using transformer models, and visualization of results.

Key features include:
- Automated abstract fetching from PubMed.
- Token-level processing for large abstracts.
- Sentiment analysis using a pre-trained BiomedBERT model.
- Visualization of analysis results.

---

## Getting Started

### Prerequisites  
Install the required dependencies using the following command:  
```
pip install -r requirements.txt  
```

---

## Project Structure

``` 
📂 Project Root  
├── 0.API_Data_Gathering.ipynb       # Script to collect PubMed abstracts using PubMed API  
├── 1.Data_Processing_and_Labelling.ipynb  # Processes abstracts and applies sentiment analysis  
├── 2.Visualization_and_Analysis.ipynb    # Analyzes labeled data and generates visualizations  
├── datasets/  
│   ├── raw_abstracts.json         # Raw abstracts gathered from PubMed  
│   ├── labeled_abstracts.parquet  # Processed and labeled abstracts  
├── requirements.txt               # Python dependencies  
├── LICENSE                        # Project license  
└── README.md                      # Project overview and instructions  
```

---

## Usage Instructions

1. **Run the data gathering script**:  
   Execute `0.API_Data_Gathering.ipynb` to fetch abstracts from PubMed. Store the data in `datasets/raw_abstracts.json`.

2. **Process and label data**:  
   Use `1.Data_Processing_and_Labelling.ipynb` to preprocess the abstracts and apply sentiment analysis. Results will be saved as `datasets/labeled_abstracts.parquet`.

3. **Analyze results**:  
   Load `2.Visualization_and_Analysis.ipynb` to visualize and analyze labeled data.

4. **Optional GitHub Pages setup**:  
   Host project documentation using GitHub Pages. Follow these steps:  
   - Push your repository to GitHub.  
   - Go to **Settings > Pages** in your repository.  
   - Under "Source," select the branch (e.g., `main`) and folder (`/docs` or `/`).  
   - Your project will be live at:  
     ///  
     https://<your-github-username>.github.io/<your-repository-name>  
     ///

---

## Contributions

Contributions are welcome! To contribute:  
1. Fork the repository.  
2. Create a branch for your feature or bug fix.  
3. Submit a pull request.

---

## License  

This project is licensed under the MIT License. See the `LICENSE` file for details.



------------------------------ 
2nd file
------------------------------

# PubMed Data Gathering, Cleaning, and Sentiment Analysis Pipeline

This project provides a comprehensive pipeline for gathering, processing, cleaning, and analyzing biomedical literature data from PubMed. It includes tools for:
- Querying and downloading PubMed articles via the PubMed API.
- Cleaning and deduplication of large datasets for high-quality processing.
- Performing auto-labeling using a pre-trained model (BioMedBERT).
- Analyzing and visualizing the labeled data.

---

## Dataset

The processed PubMed dataset used in this project is available on [Kaggle](https://kaggle.com/datasets/9896569a20d89768d95f0a4ce3fe2b459a5e36b6e2a56ef19d1b165c113be03b). It includes:
- **Raw PubMed abstracts** collected using the `0.API_Data_Gathering.ipynb` file.
- **Cleaned abstracts** processed and deduplicated for consistency.
- **Labeled abstracts** auto-labeled with sentiment using BioMedBERT.

---

## Project Structure

```
📂 Project Root
├── 0.API_Data_Gathering.ipynb # Script to collect PubMed abstracts using PubMed API
├── 1.Data_Processing_and_Labelling.ipynb # Processes abstracts and applies sentiment analysis
├── 2.Visualization_and_Analysis.ipynb # Analyzes labeled data and generates visualizations
├── Data/
│ ├── 0.Raw/
│ │ ├── API_data/results/ # Raw JSON files collected from PubMed
│ ├── 1.EarlyCleaned/
│ │ ├── raw_parquet/ # Flattened and structured Parquet files
│ │ ├── cleaned_parquet/ # Deduplicated and cleaned Parquet files
│ │ └── cleaned_parquet_final/ # Final deduplicated Parquet dataset
│ └── 2.Labeled/
│ ├── labeled_abstracts.parquet # Processed and labeled abstracts
├── requirements.txt # Python dependencies
├── LICENSE # Project license
└── README.md # Project overview and instructions
```


---

## Data Processing Overview

### 1. JSON to Parquet Conversion
The first script converts raw JSON files from PubMed into structured Parquet files:
- **Flattening nested data**: Extracts fields like `abstract_sections`, `authors`, `mesh_terms`, and `keywords` into flat columns.
- **Output**: Consolidates data into a single Parquet file (`all_results.parquet`) or separate monthly files.
- **Efficiency**: Simplifies handling of large datasets.

### 2. Data Cleaning and Deduplication
The second script ensures data quality by:
- **Standardizing publication dates** into the `YYYY-MM-DD` format.
- **Identifying and removing duplicates**:
  - **Deduplication by `uid`**: Removed **401,132 duplicate rows** based on unique identifiers.
  - **Deduplication by multi-columns**: Ensured no discrepancies between deduplication strategies.
- **Final dataset**: 1,059,761 unique rows saved as `PubMedAbstracts_final.parquet`.

### 3. Sentiment Analysis
- The processed data is auto-labeled using a pre-trained BioMedBERT model.
- Sentiment scores and labels are appended to abstracts, enabling downstream analysis.

---

## Getting Started

### Prerequisites  
Install the required dependencies using the following command:  



### Steps
1. **Run the data gathering script**:  
   Execute `0.API_Data_Gathering.ipynb` to fetch abstracts from PubMed. Store the data in `Data/0.Raw/API_data/results/`.

2. **Process and clean data**:  
   Use `1.Data_Processing_and_Labelling.ipynb` to preprocess the abstracts, remove duplicates, and apply sentiment analysis. Results will be saved as `Data/2.Labeled/labeled_abstracts.parquet`.

3. **Analyze results**:  
   Load `2.Visualization_and_Analysis.ipynb` to visualize and analyze labeled data.

4. **Optional GitHub Pages setup**:  
   Host project documentation using GitHub Pages. Follow these steps:  
   - Push your repository to GitHub.  
   - Go to **Settings > Pages** in your repository.  
   - Under "Source," select the branch (e.g., `main`) and folder (`/docs` or `/`).  
   - Your project will be live at:  
     ///
     https://<your-github-username>.github.io/<your-repository-name>  
     ///

---

## Contributions

Contributions are welcome! To contribute:  
1. Fork the repository.  
2. Create a branch for your feature or bug fix.  
3. Submit a pull request.

---

## License  

This project is licensed under the MIT License. See the `LICENSE` file for details.



------------------------------ 
3rd file
------------------------------








------------------------------
ADDITIONAL file
------------------------------
Project Description
This dataset aims to facilitate text mining and longitudinal research on biomedical publications. By focusing on disease-/health-related terms, English language, US-based affiliations, and human subjects, it spans ~1 million abstracts from 1995 to 2024.
Researchers might analyze how interest in certain illnesses changes over time, correlate publication intensity with periods of higher disease prevalence or policy changes, and develop advanced NLP methods (e.g., sentiment classification, entity recognition, topic modeling). The variety of token fields can be used to train or fine-tune domain-specific language models (such as BiomedBERT).

1) Overview
Focus: PubMed abstracts matching disease/illness queries, US affiliations, humans, in English.
Time Range: ~1995–2024, capturing changes in disease research intensity.
Size: ~1 million records, suitable for large-scale text analysis and pattern discovery (e.g., correlation with actual outbreak timelines).
2) Files & Format
We provide several Parquet files:

P5_final_new.parquet (~2.63 GB)

Core Fields:
uid (PubMed ID), title, journal, abstract, authors, affiliations, mesh_terms, keywords, coi_statement, parsed_date.
Token Fields:
cleaned_title_tokens_simple, cleaned_abstract_tokens_simple: Minimal tokenization (lowercase, bracket removal, punctuation trimming). Retains key domain terms like “mortality,” “hiv.”
disease_abstract_spacy, disease_title_spacy: Disease-centric tokens extracted via spaCy pipeline or additional heuristics. Could highlight “HIV,” “AIDS,” “cancer,” etc.
Some “_hf” fields may reflect tokenization with Hugging Face tokenizers.
Use Cases: Directly feed to your ML pipelines (e.g., classification, entity extraction).
PubMedAbstracts_final.parquet (~1.32 GB)

A somewhat smaller or differently processed subset with fewer columns (like uid, title, abstract, parsed_date).
Chunks_Abstracts/ folder

Contains chunked Parquet files (~100K rows each) if you want to process in partial loads instead of one big file.
All files are in Parquet format for more efficient reading and partial loading.

3) Token Fields Explained
During data preparation, we generated multiple token fields to accommodate different NLP approaches:

Minimal (“_simple”) Tokenization

Lowercased and stripped bracketed citations or extraneous punctuation (e.g., [1]).
Kept essential domain terms (like “immunotherapy,” “diabetes”) for analyzing disease contexts.
Splitting on spaces or basic punctuation, ensuring an accessible, “lightly cleaned” token list.
Hugging Face (“_hf”) Tokens

If present, these were produced using Hugging Face Transformers’ tokenizer.encode_plus or similar.
May have subword tokens (e.g., “immuno,” “##therapy”) typical of BERT-like models.
Suitable for direct input to a BERT-based pipeline (like BiomedBERT or distilbert-base-uncased).
SpaCy Disease-Related Tokens (“disease_title_spacy, disease_abstract_spacy`)

Used a spaCy model or custom rules to focus on disease terms, possibly leveraging en_ner_bc5cdr_md or specialized disease-entity recognition.
Helpful for quickly identifying disease mentions (e.g., “HIV,” “COVID-19,” “malaria,” “cancer”).
By providing these different token sets, you can pick whichever approach fits your workflow: minimal raw tokens for a broad approach, or specialized/disease tokens for narrower tasks.

4) Minimal Cleaning & Date Corrections
Lowercased textual fields.
Removed bracketed citations [1].
Removed extraneous punctuation or special characters, while preserving domain terms.
Corrected or approximated publication dates (parsed_date) if needed (some had None-01-01 placeholders).
5) Source & Disclaimer
Origin: PubMed (National Library of Medicine) via a refined query (disease/health terms, English language, US affiliation, humans).
License & Usage:
Journal abstracts typically remain under original author/publisher copyright.
Per NCBI disclaimers, NLM does not claim copyright on abstracts but usage beyond fair use may need copyright holder permission.
Provided: For research / text-mining only, respecting NCBI E-utilities usage guidelines.
6) Potential Applications
Longitudinal Disease Study:

Compare abstract frequencies over time, e.g., do you see a spike in “influenza” publications around outbreak years?
Evaluate correlations between publication intensity and real-world disease surges.
NLP:

Named Entity Recognition (e.g., extracting disease names, chemicals, symptoms).
Sentiment Analysis (noting that scientific abstracts are typically neutral).
Summarization or language model fine-tuning (BiomedBERT, PubMedBERT).
Bibliometric / Trend Analysis:

Evaluate how research on certain conditions has grown or shrunk over the decades, specifically for US-affiliated institutions.
Explore mesh_terms or keywords to find emergent disease topics.
Machine Learning Fine-Tuning:

The token fields let you quickly adapt domain LMs to classification or other tasks.
If your memory is limited, consider reading data from the Chunks_Abstracts/ folder in smaller batches.

