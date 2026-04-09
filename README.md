# COMP_9150_Cyber_Security_Final_Project

Dataset used: 
https://www.unb.ca/cic/datasets/dns-2021.html
Raw-20240207T040928Z-001.zip

    Raw-20240207T040928Z-001/
        ├── CSV_benign.csv
        ├── CSV_malware.csv
        ├── CSV_phishing.csv
        └── CSV_spam.csv

Processed by
    cleaning.ipynb
    preprocessing.ipynb

Model Trained by 
    Cybersecurity.ipynb

## Cybersecurity Model - What Was Done

### 1. **Data Loading & Preparation**
   - Loaded preprocessed dataset (`df_final2.csv`) from cleaning and preprocessing stages
   - Dataset contains DNS traffic features for 4 security classes: Benign, Malware, Phishing, and Spam

### 2. **Exploratory Data Analysis (EDA)**
   - Mapped numeric labels to class names for better interpretability
   - Analyzed class distribution to understand data imbalance
   - Visualized class distribution using bar charts

### 3. **Feature Engineering**
   - Categorized features into 3 types:
     - **Numerical Features** (11): ASN, TTL, subdomain, len, entropy, Domain_Age, numeric_percentage, Name_Server_Count, obfuscate_at_sign, vowel_count, vowel_ratio
     - **Categorical Features** (7): Country, State, Organization, tld, Registrar, sld, IP_Subnet
     - **Text Features** (5): Domain, Domain_Name, Registrant_Name, longest_word, Emails
   - Handled missing values: filled text/categorical features with "missing" and numerical features with 0

### 4. **Model Training**
   - **Algorithm**: CatBoost Classifier (multi-class classification) [https://catboost.ai/docs/en/]
   - **Model Configuration**:
     - 800 iterations with depth 6
     - Learning rate: 0.05
     - Auto class weight balancing (SqrtBalanced) to handle imbalanced classes
     - Text processing enabled (Naive Bayes + Bag of Words with n-grams)
   - **Train/Test Split**: 80/20 split with stratification (random_state=42)
   - **Early Stopping**: 50 rounds to prevent overfitting

### 5. **Model Evaluation**
   - Generated predictions on test set
   - Calculated and displayed:
     - Classification report (precision, recall, F1-score per class)
     - Confusion matrix for error analysis

### 6. **Risk Scoring System**
   - Created weighted probability scores combining model predictions
   - Generated risk scores (0-100) based on class weights
   - Categorized threats into risk levels:
     - **Low** (0-20): Safe/benign traffic
     - **Medium** (21-70): Suspicious but not critical
     - **High** (71-90): Likely malicious
     - **Critical** (91-100): High-confidence threats (malware)
