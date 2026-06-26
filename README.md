# SE Competency Demand in Indonesia's AI Era
### Dataset & Analysis Pipeline — Undergraduate Thesis & Journal Article

> **Associated publication:**  
> Alifah Zahro Dzakiyyah. *"Profiling AI-Era Software Engineering Competency Demand: A Data-Driven Analysis of Indonesian Job Postings."* Jurnal Teknologi Informasi dan Multimedia (JTIM), 2026.

---

## Overview

This repository contains the processed datasets and Jupyter notebook pipeline used to profile software engineering (SE) competency demand in Indonesia using job posting data. The study scraped 4 major job platforms, extracted 209 unique hard skills using an ESCO-based bilingual dictionary, and applied TF-IDF + K-Means clustering to identify 9 distinct SE specialization profiles.

---

## Repository Structure

```
se-competency-indonesia/
├── data/
│   ├── raw/                        # Raw scraped CSVs (see note below)
│   ├── processed/
│   │   ├── jobs_cleaned_v2.csv     # 669 postings after cleaning & dedup
│   │   ├── jobs_with_skills.csv    # 402 SE-scoped postings + extracted skills
│   │   └── jobs_clustered.csv      # 337 postings + cluster labels (K=9)
│   └── tables/
│       ├── skill_frequency_table.xlsx   # Frequency of 209 unique skills
│       └── cluster_summary_table.xlsx   # Profile summary of 9 clusters
├── notebooks/
│   ├── 01_data_cleaning_v2.ipynb
│   ├── 02_skill_extraction_v3.ipynb
│   └── 03_tfidf_clustering_final.ipynb
├── LICENSE
└── README.md
```

> **Note on raw data:** Raw CSV files from Glassdoor, Indeed, JobStreet, and LinkedIn are not included due to platform Terms of Service. Data was collected via Apify on 3 June 2026.

---

## Data Collection

| Platform  | Scraper              | Collection Date     |
|-----------|----------------------|---------------------|
| Glassdoor | Glassdoor Jobs       | 3 June 2026         |
| Indeed    | Indeed Scraper       | 3 June 2026         |
| JobStreet | JobStreet Scraper    | 3 June 2026         |
| LinkedIn  | LinkedIn Job Scraper | 3 June 2026         |

**Query:** Software engineering roles in Indonesia  
**Total raw postings collected:** varies per platform (merged & deduplicated to 669)

---

## Pipeline Summary

```
[4 Raw CSVs] 
    → NB1: Cleaning & Deduplication → jobs_cleaned_v2.csv (n=669)
    → NB2: SE Scope Filter (SWEBOK v4) + Skill Extraction → jobs_with_skills.csv (n=402)
    → NB3: TF-IDF Vectorization + K-Means (K=9) → jobs_clustered.csv (n=337)
```

### Notebook 1 — Data Cleaning (`01_data_cleaning_v2.ipynb`)
- Standardizes column schemas across 4 platforms
- Removes duplicates via cross-platform deduplication
- Drops records with missing `job_description`
- **Output:** `jobs_cleaned_v2.csv` (669 postings)

### Notebook 2 — Skill Extraction (`02_skill_extraction_v3.ipynb`)
- Filters SE-scope postings using SWEBOK v4 Knowledge Areas + SFIA 9 role taxonomy
- Extracts hard skills using a validated bilingual (EN/ID) ESCO-based dictionary
- Covers 209 unique skills across 8 categories (Programming Language, Framework & Library, Cloud & DevOps, Database, AI/ML, QA & Testing, Architecture, Soft Skill)
- Computes AI skill prevalence per posting
- **Output:** `jobs_with_skills.csv` (402 postings), `skill_frequency_table.xlsx`

### Notebook 3 — Clustering (`03_tfidf_clustering_final.ipynb`)
- TF-IDF vectorization on extracted skill tokens
- Elbow Method + Silhouette Score + Davies-Bouldin Index to determine optimal K
- K-Means clustering with K=9 (Silhouette Score: 0.0493, DBI: optimal)
- Cluster labeling mapped to SWEBOK v4 Knowledge Areas and SFIA 9 levels
- **Output:** `jobs_clustered.csv` (337 postings), `cluster_summary_table.xlsx`

---

## Dataset Schema

### `jobs_with_skills.csv`
| Column | Description |
|--------|-------------|
| `job_title` | Job posting title |
| `company` | Company name |
| `location` | Job location |
| `sumber` | Source platform (Glassdoor / Indeed / JobStreet / LinkedIn) |
| `job_description` | Full job description text |
| `skills_list` | Extracted skills as a list |
| `skills_str` | Extracted skills as space-separated string |
| `skill_count` | Number of skills detected |
| `is_ai_job` | Boolean: whether posting requires AI/ML skill |
| `language` | Detected language of posting (en / id) |

### `jobs_clustered.csv`
All columns from `jobs_with_skills.csv`, plus:

| Column | Description |
|--------|-------------|
| `cluster` | K-Means cluster label (0–8) |
| `cluster_name` | Named cluster (e.g., "AI/ML & LLM Engineering") |

---

## Cluster Profiles (K=9)

| Cluster | Name | n | % of Total | AI Prevalence |
|---------|------|---|------------|---------------|
| 0 | Enterprise Dev & Agile Practices | 74 | 22.0% | 28% |
| 1 | .NET & C-Family Systems Development | 36 | 10.7% | 19% |
| 2 | DevOps & Container Orchestration | 39 | 11.6% | 33% |
| 3 | QA & Test Automation | 33 | 9.8% | 24% |
| 4 | Web Backend & Database (PHP/Laravel) | 28 | 8.3% | 21% |
| 5 | Multi-Cloud Platform Engineering | 50 | 14.8% | 36% |
| 6 | Full-Stack & Microservices (Go/React) | 30 | 8.9% | 20% |
| 7 | AI/ML & LLM Engineering | 31 | 9.2% | 77% |
| 8 | Mobile App Development (Kotlin/Flutter) | 16 | 4.7% | 19% |

---

## Frameworks & Standards Used

- **SWEBOK v4.0** (IEEE Computer Society) — for SE scope definition and cluster mapping
- **SFIA 9** (Skills Framework for the Information Age) — for competency level profiling
- **ESCO v1.2.1** (European Skills, Competences, Qualifications and Occupations) — as basis for skill dictionary
- **SKKNI** — for alignment with Indonesian national competency standards

---

## Requirements

```bash
pip install pandas numpy scikit-learn matplotlib seaborn wordcloud openpyxl langdetect beautifulsoup4 ftfy emoji lxml
```

Python 3.9+

---

## Citation

If you use this dataset or code, please cite:

```bibtex
@article{dzakiyyah2026se,
  title     = {Profiling AI-Era Software Engineering Competency Demand: A Data-Driven Analysis of Indonesian Job Postings},
  author    = {Dzakiyyah, Alifah Zahro},
  journal   = {Jurnal Teknologi Informasi dan Multimedia (JTIM)},
  year      = {2026}
}
```

---

## License

Data and code are released under the [MIT License](LICENSE).  
Raw job posting content remains subject to the Terms of Service of the respective platforms.

---

## Contact

**Alifah Zahro Dzakiyyah**  
Program Studi Sistem Informasi — STT Terpadu Nurul Fikri  
