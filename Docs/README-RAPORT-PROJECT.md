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
## **1. API Data Gathering**
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

## **2. Parquet Early Data Cleaning**
### **Purpose**
[1.Parquet_Early_Data_Cleaning.ipynb](https://github.com/MPKuchciak/PubMedResearch/blob/main/Notebooks/1.Parquet_Early_Data_Cleaning.ipynb)  
### **Purpose**
This Jupyter Notebook is designed to process and clean a collection of JSON files containing article data.
- Load raw JSON data containing PubMed abstracts, flatten the nested structures, and convert the data into Parquet format for efficient storage and processing.
- Address inconsistencies in publication date formats and eliminate duplicate articles.

### **Conclusions**
1. The downloaded data contained 401,132 duplicate rows, accounting for 27.5% of the dataset.
2. The final dataset used for analysis contains 1,059,761 unique rows after deduplication.


---

## **3. Exploratory Data Analysis (EDA)**
### **Purpose**
Provides an **initial statistical and visual exploration** of the dataset.

### **Main Analyses Performed**
1. **Basic Descriptive Statistics**:
   - Number of articles.
   - Distribution of abstracts across journals.
   - Missing values and data consistency.
2. **Temporal Trends**:
   - Articles per **year** to see **publication growth**.
3. **Journal Distribution**:
   - Identifies the most **frequent journals**.
4. **Keyword and MeSH Term Distributions**:
   - Counts **most common** terms.
   - Analyzes **disease topics**.
5. **Abstract Length Distribution**:
   - Evaluates the **length of abstracts** to determine variability.

### **Insights**
- Identifies potential **biases in publication trends**.
- Highlights **which topics are most studied**.

### **Next Steps**
- Deeper **sentiment** and **topic modeling** analysis.
- **Co-authorship network analysis**.

---

## **4. EDA Tokenization**
### **Purpose**
This notebook focuses on **tokenizing abstracts and titles** to extract **meaningful words** for further NLP tasks.

### **Tokenization Methods Used**
1. **Simple Tokenization**:
   - Splits text into words.
   - Removes **stopwords, punctuation, and numbers**.
2. **Hugging Face-based Tokenization**:
   - Uses **pretrained models (e.g., BERT, SciBERT)** for advanced **subword tokenization**.
3. **spaCy Tokenization**:
   - Biomedical-focused **named entity recognition (NER)**.
4. **MeSH Term Matching**:
   - Ensures **standardized medical terminology**.

### **Additional Processing**
- **Stemming/Lemmatization**: Converts words to their base form.
- **Entity Recognition**: Detects **disease-related terms**.
- **Part-of-Speech (POS) Tagging**: Helps identify **nouns, verbs, adjectives**.

### **Impact**
- Creates a **clean tokenized dataset**.
- Prepares data for **disease frequency analysis and topic modeling**.

---

## **5. Keywords Analysis**
### **Purpose**
Analyzes **trends in research article keywords** to identify **popular and emerging topics**.

### **Key Steps**
1. **Loading Data**:
   - Reads cleaned data containing **article keywords**.
2. **Keyword Processing**:
   - Splits **semicolon-separated keywords**.
   - Converts to **lowercase** and removes **duplicates**.
3. **Frequency Analysis**:
   - Uses `Counter()` from Python’s `collections` module.
   - Identifies **most frequently used keywords**.
4. **Temporal Trends**:
   - Tracks **how keyword popularity changes over time**.
   - **Plots keyword trends** (line plots over publication years).

### **Key Insights**
- Identifies **most popular biomedical research topics**.
- Tracks **emerging research areas**.

### **Potential Next Steps**
- **Comparison with disease mentions** from abstracts.
- **Clustering similar keywords** using NLP techniques.

---

## **6. MeSH Terms Analysis**
### **Purpose**
This notebook examines **MeSH (Medical Subject Headings)** terms in PubMed abstracts.

### **Why MeSH Matters**
- MeSH terms **standardize biomedical indexing**, ensuring that:
  - Different synonyms (e.g., **"Heart Attack" vs. "Myocardial Infarction"**) map to the **same concept**.
  - Researchers **retrieve more relevant papers**.

### **Key Analyses**
1. **Loading and Filtering MeSH Terms**:
   - Extracts **MeSH term column** from the dataset.
2. **Frequency Distribution**:
   - Uses `Counter()` to find **most frequently used MeSH terms**.
3. **Comparing MeSH Terms with Keywords**:
   - Checks **overlap** between **MeSH terms and author-defined keywords**.
4. **Temporal Analysis**:
   - Tracks **trends in disease research focus** over time.

### **Potential Insights**
- Identifies **most researched diseases**.
- Tracks **how focus on specific medical conditions shifts**.

### **Next Steps**
- **Topic modeling** to see how MeSH terms **relate to research themes**.
- **Combining with sentiment analysis** to see **how certain diseases are discussed**.

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
