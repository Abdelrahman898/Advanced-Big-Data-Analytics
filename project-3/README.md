## Clustering Analysis on Open Research Dataset CORD 19  

#### Overview

In response to the COVID-19 pandemic, the White House and a coalition of leading research groups have prepared the COVID-19 Open Research Dataset (CORD-19). CORD-19 is a resource of over 57,000 scholarly articles, including over 45,000 with full text, about COVID-19, SARS-CoV-2, and related coronaviruses. This freely available dataset is provided to the global research community. As a big data community, how can we help researchers to easily find the related research papers easily?  

You can find the dataset and the main challenge on [Kaggle](https://www.kaggle.com/allen-institute-for-ai/CORD-19-research-challenge)

---

#### Goals  

Given the large number of literature and the rapid spread of COVID-19, it is difficult for health professionals to keep up with new information on the virus. Can clustering similar research articles together simplify the search for related publications? How can the content of the clusters be qualified? And over each cluster, how can we recommend the most similar papers leveraging clustering?  

---

#### Requirements 

1. Cluster research papers using the JSON files (research papers details) and metadata CSV file.  
2. Build a neighborhood recommender system to receive a paper title and recommend the top N most similar papers based on its cluster.  
3. Represent papers as vectors, cluster them, and build the recommender system.  

---

#### Steps  

##### 1. Read the Dataset Using Spark

Use Databricks.com with pre-mounted data paths: 

- `comm_use_subset_path = "/databricks-datasets/COVID/CORD-19/2020-03-13/comm_use_subset/comm_use_subset/"`  

- `noncomm_use_subset_path = "/databricks-datasets/COVID/CORD-19/2020-03-13/noncomm_use_subset/noncomm_use_subset/"`  

- `biorxiv_medrxiv_path = "/databricks-datasets/COVID/CORD-19/2020-03-13/biorxiv_medrxiv/biorxiv_medrxiv/"`  

- `json_schema_path = "/databricks-datasets/COVID/CORD-19/2020-03-13/json_schema.txt"`  

---

##### 2. Exploratory Data Analysis (EDA)  

- Perform EDA to understand data structure and extract insights for feature engineering.  
- Document findings.  

---

##### 3. Preparation and Cleaning  

- Join JSON files with CSV metadata.  
- Handle nulls, duplications, and non-English documents (use language detection libraries).  

---

##### 4. Preprocessing  

using Spark ML:

1. Remove stop words.  
2. Remove custom stop words:  
   ```python  
   custom_stop_words = ['doi', 'preprint', 'copyright', 'peer', 'reviewed', 'org', 'https', 'et', 'al', 'author', 'figure', 'rights', 'reserved', 'permission', 'used', 'using', 'biorxiv', 'medrxiv', 'license', 'fig', 'al.', 'Elsevier', 'PMC', 'CZI', 'www']  
   ```  
3. Remove punctuation using regex: `[!()\-\[\]{};:'"\\,<>./?@#$%^&*_~]`  
4. Convert text to lowercase.  

---

##### 5. Vectorization  

- Use **TF-IDF** or **Word2Vec** for vector representation.  
- Resources:  
  - [Spark ML documentation](https://spark.apache.org/docs/latest/mllib-feature-extraction.html)  
  - [TF-IDF tutorial](https://www.youtube.com/watch?v=hc3DCn8viWs)
  - [Word2Vec tutorial](https://www.youtube.com/watch?v=3eoX_waysy4)  

---

##### 6. Clustering  

- Apply clustering algorithms (e.g., K-means).  
- Use the Elbow Method to determine optimal *k*.  
- Use PCA to reduce dimensions (retain 95% variance).  
- Evaluate and justify algorithm choice.  

---

##### 7. Recommender System

- Build a function `recommendPaper(paper_title, N)` that:  
  - Returns top N recommendations based on cosine similarity within the cluster.  

---
