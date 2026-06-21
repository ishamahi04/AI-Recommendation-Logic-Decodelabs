 # 🤖 Tech Stack Recommender — DecodeLabs AI Project 3

> **Industrial Training Kit · Batch 2026 · Powered by DecodeLabs**

A content-based filtering recommendation engine that maps a user's raw skills and career goals to the most relevant job roles using **TF-IDF vectorisation** and **Cosine Similarity** — no neural networks, no historical data, pure algorithmic logic.

---

## 📋 Project Overview

| Field | Details |
|-------|---------|
| **Project** | AI Project 3 — AI Recommendation Logic |
| **Track** | Personalization / Pattern Alignment |
| **Algorithm** | Content-Based Filtering (TF-IDF + Cosine Similarity) |
| **Input** | Minimum 3 user skills |
| **Output** | Top-3 ranked job role recommendations |

---

## 🧠 How It Works — The IPO Pipeline

```
USER SKILLS INPUT  →  TF-IDF VECTORISATION  →  COSINE SIMILARITY SCORING  →  TOP-N LIST
```

### Step 1 — Ingestion
Capture at least **3 user skills** to ensure sufficient data density for accurate matching.

### Step 2 — Scoring (TF-IDF + Cosine Similarity)
- Each job role and the user profile are converted into **TF-IDF weighted vectors** in a shared vocabulary space.
- **Term Frequency (TF):** Terms appearing more within a role are weighted higher.
- **Inverse Document Frequency (IDF):** Generic terms (appearing across all roles) are penalised.
- **Cosine Similarity** measures the angular alignment between user vector and each role vector — magnitude-invariant and scale-agnostic.

```
cos(θ) = (A · B) / (||A|| × ||B||)
```

| Score | Meaning |
|-------|---------|
| `1.0` | Perfect match — identical orientation |
| `0.5` | Moderate match |
| `0.0` | No common features (orthogonal vectors) |

### Step 3 — Sorting
Results ranked in **descending order** by cosine similarity score.

### Step 4 — Filtering
Output truncated to the **Top-N list** (default: Top 3) to prevent choice overload.

---

## 📁 Project Structure

```
project-3/
│
├── tech_stack_recommender.py   # ← Main script (all logic lives here)
├── raw_skills.csv              # Dataset: job roles + required skills
├── README.md                   # You are here
└── requirements.txt            # Python stdlib only — no installs needed
```

---

## ⚙️ Setup & Usage

### Prerequisites
- Python 3.10 or higher
- No external libraries required (uses `math`, `csv`, `collections` from stdlib)

### Run interactively
```bash
python tech_stack_recommender.py
```
You will be prompted:
```
Enter your skills (minimum 3, separated by commas).
Example:  Python, Machine Learning, SQL

Your skills: Python, Cloud, Docker
```

### Run with CLI arguments
```bash
python tech_stack_recommender.py --skills "Python" "SQL" "Machine Learning"
```

### Custom dataset & Top-N
```bash
python tech_stack_recommender.py \
  --skills "AWS" "Docker" "Linux" \
  --csv my_dataset.csv \
  --top 5
```

### Sample Output
```
============================================================
  TOP RECOMMENDATIONS
============================================================
  Based on your skills: Python, Machine Learning, SQL

  #1  Data Scientist
       Match  : 35.9%  ███████
       Skills : Python, SQL, Machine Learning, Data Analysis, Statistics

  #2  AI Engineer
       Match  : 27.1%  █████
       Skills : Python, Machine Learning, Deep Learning, TensorFlow, NLP

  #3  Full Stack Developer
       Match  : 10.2%  ██
       Skills : JavaScript, React, Node.js, SQL, Python
============================================================
```

---

## 📊 Dataset Format (`raw_skills.csv`)

```csv
Role,Skills
Data Scientist,"Python, SQL, Machine Learning, Data Analysis, Statistics"
DevOps Engineer,"AWS, Docker, Kubernetes, CI/CD, Git, Linux"
Frontend Developer,"HTML, CSS, JavaScript, React, TypeScript"
```

> **If `raw_skills.csv` is not found**, the script automatically falls back to a built-in 12-role embedded dataset so you can run it instantly.

---

## ⚠️ DOs and DON'Ts

### ✅ DOs
- **DO** enter at least 3 skills — fewer skills reduce matching accuracy
- **DO** use consistent skill naming (e.g. "Machine Learning" not "ML") — vocabulary must match exactly
- **DO** expand your dataset with more roles for better coverage
- **DO** treat every low-score result as a learning opportunity — check what skills are missing
- **DO** experiment with `--top` to explore more recommendations

### ❌ DON'Ts
- **DON'T** use inconsistent naming — "Web Design" ≠ "Frontend Development" in vector space
- **DON'T** confuse this with Collaborative Filtering — this system has no user-user comparison
- **DON'T** expect high scores with very generic skills (e.g. "software", "computer") — IDF penalises them intentionally
- **DON'T** ignore the Cold Start warning — it means your skill names don't exist in the vocabulary

---

## 🔑 Key Concepts Demonstrated

| Concept | Where It Appears |
|---------|-----------------|
| Content-Based Filtering | Core recommendation paradigm |
| TF-IDF Weighting | `compute_tf()` + `compute_idf()` |
| Cosine Similarity | `cosine_similarity()` |
| Cold-Start Handling | Zero-vector guard in scoring + warning display |
| Top-N Filtering | `recommend()` — `scored[:top_n]` |
| Input-Process-Output model | Full 4-step pipeline in `recommend()` |

---

## 🚫 Why NOT Euclidean Distance?

Euclidean distance is **sensitive to vector magnitude**. A role with 20 skills would appear "far" from a user with 3 skills even if those 3 skills are a perfect subset. Cosine similarity only cares about **direction** (orientation of preferences), not size — making it the industry standard for text and feature-based matching.

---

## 🐛 Common Issues

| Issue | Fix |
|-------|-----|
| Match scores are all 0% | Your skill names don't match any vocabulary term — check spelling |
| Only 1–2 results shown | Fewer than `top_n` roles scored above 0 |
| `raw_skills.csv` not found | File will use embedded dataset automatically |
| Less than 3 skills error | The script enforces a minimum of 3 skills |

---

## 🚀 Extension Ideas (For Extra Credit)

- Add a **user rating feedback loop** to re-weight preferences
- Implement **Jaccard Similarity** as a fallback for binary feature sets
- Build a **Flask/Streamlit web UI** around `recommend()`
- Add a **Cold-Start onboarding survey** for new users with no skill history
- Expand dataset with **100+ roles** from job boards

---

## 👨‍💻 Author

Developed as part of the **DecodeLabs AI Industrial Training Programme**
**Batch 2026** | Project 3 — AI Recommendation Logic

---

*"Before you build complex neural collaborative filtering models, you must master the fundamental art of matching user profiles with item attributes using pure similarity logic."* — DecodeLabs
