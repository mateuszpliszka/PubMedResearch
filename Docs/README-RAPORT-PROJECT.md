# The Most Popular Diseases in Medical Articles on PubMed

Authors:
- Maciej Kuchciak
- Mateusz Pliszka
- Łukasz Janisiów

## Project Overview:
The main goal of this analysis is to identify the most popular diseases in medical articles on PubMed from January 1994 to December 2024. The analysis includes more than 1 million articles, focusing primarily on titles, abstracts, and MeSH terms. 

Additionally, the analysis investigates:
- Sentiment of abstracts
- Network between authors and their co-authors
- Connection between COVID-19 infection rates and the number of COVID-19 related articles
- Exploratory Data Analysis to find others interesting insights from data

Our analysis aims to answer the following questions:
- What is the most popular illness describe in medical articles from 1994 to 2024? How does it vary each year?
- Is there a correlation between COVID-19 cases and the number of related articles?
- Is there a noticeable sentiment in abstracts? Does it change over time?
- Are there any other features that change over time in scientific articles?
---
## **0. API Data Gathering**
### **File** 
[0.API_Data_Gathering.ipynb](https://github.com/MPKuchciak/PubMedResearch/blob/main/Notebooks/0.API_Data_Gathering.ipynb)  
### **Purpose**
This Jupyter Notebook is designed to fetch and process PubMed articles using the PubMed API. It provides a complete pipeline for querying PubMed, fetching article details, and saving the data for further analysis.

The following types of data are downloaded for each PubMed article:
   - uid: The unique PubMed ID (PMID) of the article.
   - title: The title of the article. 
   - journal: The name of the journal in which the article was published. 
   - pubdate: The publication date of the article in the format "YYYY-MM-DD".
   - abstract_sections: A list of sections within the article's abstract, including labels, categories, and text.
   - authors: A list of authors, including their names, initials, ORCID IDs, and affiliations.
   - mesh_terms: Medical Subject Headings (MeSH) terms associated with the article, including descriptors, major topics, and qualifiers.
   - keywords: Keywords associated with the article.

### **Conclusions**
1. The total number of articles downloaded is 1,460,893.

---

## **1. Parquet Early Data Cleaning**
### **File**
[1.Parquet_Early_Data_Cleaning.ipynb](https://github.com/MPKuchciak/PubMedResearch/blob/main/Notebooks/1.Parquet_Early_Data_Cleaning.ipynb)  
### **Purpose**
This Jupyter Notebook is designed to process and clean a collection of JSON files containing article data.
- Load raw JSON data containing PubMed abstracts, flatten the nested structures, and convert the data into Parquet format for efficient storage and processing.
- Address inconsistencies in publication date formats and eliminate duplicate articles.

### **Conclusions**
1. The downloaded data contained 401,132 duplicate rows, accounting for 27.5% of the dataset.
2. The final dataset used for analysis contains 1,059,761 unique rows after deduplication.


---

## **2. Exploratory Data Analysis (EDA)**
### **File**
[2.EDA.ipynb](https://github.com/MPKuchciak/PubMedResearch/blob/main/Notebooks/2.EDA.ipynb)  

### **Purpose**
In this file, we perform Exploratory Data Analysis. We answered to the following questions:

- Who is the most active publisher?
- What is the most popular journal?
- Is the number of co-authors growing?
- Are there other trends in the articles?

### **Conclusions**
1. An increasing trend is observed across various domains, including the number of articles published per year, but also the number of authors per article, the number of words in abstracts, and the number of words per title.
2. The highest average number of articles are published in January, which aligns with the findings from the article "April publishing lull follows end-of-year academic flurry" [1]. This study stated that January and November are the most popular months for publishing.
3. The number of authors per article is on the rise, increasing from an average of approximately 5 authors per article in 1995 to around 11 authors per article in 2024. The overall average number of authors per article from 1995 to 2024 is 6.54.
4. The average number of words in abstracts has increased from slightly below 190 in 1995 to over 230 by 2024. The overall average number of words in abstracts for the entire period is 210.
5. The average number of words in titles has been increasing over the years, rising from 11.5 in 1995 to just over 14 in 2024. This represents an approximate 20% increase over 29 years, mirroring the same percentage increase observed in the length of abstracts.
6. The top publisher in our dataset is David A. Bennett [2], who has authored over 700 filtered articles related to diseases. In his Google Scholar profile, he has 1195 articles.
7. The most popular journal in our dataset is "Scientific Reports" with 8,778 publications, followed by "Clinical Infectious Diseases" with 5,953 publications, and "Nature Communications" with 5,537 publications. The least popular journal among the top 10 is "Proceedings of the National Academy of Sciences of the United States of America" with 3,537 publications.

[1] https://www.nature.com/nature-index/news/april-publishing-lull-follows-end-of-year-academic-flurry

[2] https://scholar.google.com/citations?user=m_NIro4AAAAJ&hl=en


---

## **3. Tokenization**
### **File**
[3.Tokenization.ipynb](https://github.com/MPKuchciak/PubMedResearch/blob/main/Notebooks/3.Tokenization.ipynb)  

### **Purpose**
This notebook processes and analyzes PubMed abstracts and titles to detect diseases using various tokenization methods and SciSpacy's biomedical NER model.

Tokenization Methods:
1. **Simple Tokenization**: Basic whitespace and punctuation-based tokenization.
2. **Hugging Face Tokenization**: Tokenization using the Hugging Face `distilbert-base-uncased` model.
3. **Dictionary-Based Disease Detection**: Filtering tokens based on a predefined disease dictionary.
4. **SciSpacy Biomedical NER**: Using the `en_ner_bc5cdr_md` model to detect disease entities.

### **Conclusions/Final Decision**
The goal was to ensure high-quality data for further analysis and modelling while retaining as much relevant information as possible.

**Choices Considered**:

Title only: Retain rows with illness-related tokens in the title.
*Result*: ~1,000,000 → 480,000 rows removed (~520,000 retained).

- Abstract + Title: Retain rows with tokens in either title or abstract.
*Result*: ~1,000,000 → 84,000 rows removed (~916,000 retained).

- Abstract + Title + MeSH terms: Include tokens found in MeSH terms.
*Result*: ~1,000,000 → 60,000rows removed (~940,000 retained).

- Abstract + Title + MeSH + Hybrid Filtering: Use advanced filtering (e.g., hybrid matching and simpler keyword matching).
*Result*: ~1,000,000 → 54,000 rows removed (~946,000 retained).

- Retain All Rows: Do not remove rows based on token presence.

**Final Decision**:
We chose Option 5 to retain all rows. This decision was based on the reasoning that data lacking illness-related tokens is not inherently irrelevant and there may be possibility to check these inputs (articles) and see what's the problem with them. 

By keeping all rows we additionaly:

- We maintain the integrity and diversity of the dataset.

- Downstream analyses remain flexible, as token filtering can be performed at a later stage if necessary.

- It enables broader exploration and modeling possibilities without prematurely discarding data.

- By taking this approach, the dataset remains inclusive, ensuring no potential insights are lost. This aligns with our goal of providing a comprehensive resource for PubMed data analysis.

---

## **4. 4.Sentiment abstract**
### **File**
[3.Tokenization.ipynb](https://github.com/MPKuchciak/PubMedResearch/blob/main/Notebooks/3.Tokenization.ipynb) 

### **Purpose**
Lorem ipsum

### **Conclusions**
Lorem ipsum
---

## **5.1 Keywords Analysis**
### **File**
[5.1.Keywords_analysis.ipynb](https://github.com/MPKuchciak/PubMedResearch/blob/main/Notebooks/5.1.Keywords_analysis.ipynb) 

### **Purpose**
This notebook analyzes keyword trends in articles to identify disease patterns over time.

### **Conclusions**
1. There is a lack of articles with keywords before 2012.
2. Keywords are often associated with names or types of diseases.
3. COVID-19 and SARS-CoV-2 have dominated the keyword trends after 2019.
4. Alzheimer's disease has shown the most rapid growth in recent years, appearing in keywords less than 100 times in 2014 and more than 800 times in 2024.
5. Epidemiology and inflammation have also shown significant growth, peaking in 2021, likely due to the high number of articles associated with COVID-19.
6. Other frequently occurring disease-related keywords exhibit a more consistent growth trend, often peaking in 2021, which is most likely associated with the highest number of articles in that year.

---

## **7. Sentiment Analysis of Medical Abstracts (Version 1)**
### **Purpose**
This notebook sets the foundation for **analyzing sentiment in scientific abstracts**, particularly from medical research articles. The core assumption is that most abstracts should be **neutral**, given the objective nature of scientific writing. However, this needs empirical verification.

### **Data Preprocessing**
- The dataset (`PubMedAbstracts_final.parquet`) is loaded and examined.
- **Missing data handling**:
  - Identifies and removes abstracts that are missing (verified to be absent from the original articles rather than lost due to processing errors).
- **Filtering out problematic data**:
  - Excludes abstracts from **1994** and **2025** (likely due to low counts or inconsistencies).
- **Feature selection**:
  - Retains relevant columns for further analysis, including:
    - **Title**
    - **Journal**
    - **Authors**
    - **MeSH terms (Medical Subject Headings)**
    - **Keywords**
    - **Parsed Date**
    - **Conflict of Interest statements (COI)**

### **What This Notebook Achieves**
- It **cleans** and **prepares** the data but does **not** yet perform sentiment analysis.
- The filtering ensures that only **valid abstracts** are included in the next steps.

### **Limitations and Next Steps**
- The notebook does **not** yet provide sentiment insights.
- Future steps include **applying NLP models** to classify abstracts based on their sentiment.

---

## **8. Sentiment Analysis of Medical Abstracts (Version 2)**
### **Purpose**
This version builds on **Version 1**, applying **sentiment analysis** to the cleaned abstracts using **VADER (Valence Aware Dictionary and sEntiment Reasoner)**.

### **Methodology**
- **Sentiment Model Used**: **VADER (from NLTK)**
- **Why VADER?**
  - Designed for **short texts** like tweets or abstracts.
  - Handles **both neutral and emotional** content.
  - Returns **four sentiment scores**:
    1. **Negative (`neg_vader`)** – Proportion of negative words.
    2. **Neutral (`neu_vader`)** – Proportion of neutral words.
    3. **Positive (`pos_vader`)** – Proportion of positive words.
    4. **Compound (`compound_vader`)** – A single overall sentiment score.

### **Implementation**
- The script applies **VADER sentiment analysis** to each abstract.
- Uses **`swifter`** for parallel processing (to speed up computations on large datasets).
- Calculates **average sentiment scores** across all abstracts.

### **Potential Insights**
1. **Most abstracts should be neutral**:
   - A high `neu_vader` score would confirm this assumption.
2. **Some abstracts may be strongly positive or negative**:
   - Identifying these cases might reveal interesting patterns.
   - Are **negative abstracts** about diseases with poor prognoses?
   - Are **positive abstracts** about breakthrough treatments?

### **Possible Next Steps**
- **Visualization of sentiment distribution** (e.g., histograms of compound sentiment scores).
- **Comparison across diseases**: Are some medical conditions associated with more positive/negative sentiment?
- **Temporal analysis**: Did sentiment shift over time?

---

## **9. Topic Modeling and Disease Frequency Analysis**
### **Purpose**
This notebook focuses on **identifying frequently mentioned diseases** in the abstracts.

### **Methods Used**
- **Named Entity Recognition (NER)** is performed using multiple approaches:
  - **spaCy**
  - **Hugging Face models**
  - **Dictionary-based methods**
- Each approach extracts **disease-related terms** from:
  - **Abstracts**
  - **Titles**
  - **MeSH terms (Medical Subject Headings)**

### **Data Preprocessing**
- The extracted tokens are **merged** to create unified columns:
  - **`merged_abstract_tokens`** → Diseases found in abstracts.
  - **`merged_title_tokens`** → Diseases found in titles.
  - **`merged_mesh_tokens`** → Diseases found in MeSH terms.
  - **`merged_all_tokens`** → Combined list from all sources.

### **Disease Frequency Analysis**
- The script uses Python’s `Counter` to count **how often each disease is mentioned**.
- The **top 40 diseases** by frequency are displayed.

### **Potential Insights**
1. **Which diseases are most commonly studied?**
   - Are some diseases **overrepresented or underrepresented** in medical literature?
2. **Shifts in disease focus over time**:
   - By analyzing data over different years, trends in disease research can be identified.
3. **Comparing disease frequency with sentiment analysis**:
   - Are some diseases associated with **more positive or negative abstracts**?

### **Possible Next Steps**
- **Visualization of disease frequency** (bar charts, word clouds).
- **Time-series analysis**: How has interest in certain diseases changed?
- **Comparisons with real-world prevalence** (e.g., are research trends aligned with actual disease burden?).

---

## **10. COVID-19 Analysis**
### **Purpose**
This notebook explores the relationship between **COVID-19 research publications** and **real-world COVID-19 case trends**.

### **Data Sources**
- **PubMed abstracts** related to COVID-19.
- **COVID-19 case data** from **Our World in Data (`owid-covid-data.csv`)**.

### **Key Analyses**
1. **Filtering COVID-19 research articles**:
   - Selects abstracts that contain **COVID-19-related MeSH terms**.
2. **Reading COVID-19 case data**:
   - Uses external pandemic statistics (cases, deaths).
3. **Potential correlation analysis** (not yet implemented):
   - Compare **research output** to **case trends**.

### **Potential Insights**
- **Did research output peak after major outbreaks?**
- **Was there a delay between case spikes and research publications?**
- **What COVID-19 topics were studied the most?**

### **Possible Next Steps**
- **Time-series visualization**: Overlaying research volume and COVID-19 cases.
- **Topic modeling on COVID-19 abstracts**: Identifying common research themes.
- **Comparing different countries**: Were some nations more active in COVID-19 research?

---

## **11. Network Analysis of Co-Authorship**
### **Purpose**
This notebook explores **collaboration networks** among medical researchers.

### **Methodology**
- Uses **NetworkX** to create a **graph of co-authorship**:
  - **Nodes** = Authors
  - **Edges** = Co-authored papers
- Identifies **Top 10 Most Published Authors**.

### **Findings**
- **David A. Bennett** has **700+ disease-related publications**.
- Identifies other **highly collaborative researchers**.

### **Potential Insights**
- **Who are the most influential researchers?**
- **Which researchers frequently collaborate?**
- **Are there isolated researchers?**

### **Possible Next Steps**
- **Visualizing the network** (graph plots).
- **Clustering analysis**: Identifying research groups.
- **Topic-based author analysis**: Which diseases do different clusters study?

---
# **Final Thoughts**

## **Overall Summary**


### **Early Stage Data Processing**
| Notebook | Purpose | Key Findings |
|----------|---------|-------------|
| **API Data Gathering** | Retrieves PubMed data via API. | Stores abstracts, titles, authors, MeSH terms, and keywords. |
| **Parquet Early Data Cleaning** | Cleans and preprocesses raw data. | Removes missing values, filters years, selects key columns. |
| **EDA** | Explores dataset structure. | Analyzes trends in publication years, journals, and abstract lengths. |
| **EDA Tokenization** | Processes text for NLP analysis. | Tokenizes abstracts using spaCy, Hugging Face, and dictionary-based methods. |
| **Keywords Analysis** | Examines keyword trends. | Identifies most common and rising research topics. |
| **MeSH Terms Analysis** | Analyzes standardized biomedical indexing. | Tracks research focus over time using MeSH classifications. |


### **Later Stage Analysis**
| Notebook | Purpose | Key Findings |
|----------|---------|-------------|
| **Sentiment Analysis v1** | Data preparation for sentiment analysis. | Filters abstracts, removes inconsistencies. |
| **Sentiment Analysis v2** | Applies sentiment analysis (VADER). | Abstracts likely mostly neutral, but some variation exists. |
| **Topic Modeling** | Identifies most frequently mentioned diseases. | Lists top 40 diseases, potential for trend analysis. |
| **COVID-19 Analysis** | Examines COVID-19 research trends vs. real-world cases. | Plans to correlate research volume with case numbers. |
| **Network Analysis** | Investigates co-authorship networks. | Identifies top authors, including David A. Bennett. |

---
