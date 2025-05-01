# ATS Tracker using Gemini 1.5 Pro and Streamlit

This project is an **AI-powered Applicant Tracking System (ATS)** that uses the **Gemini 1.5 Pro model** (via Google Generative AI API) to analyze and evaluate resumes against job descriptions. It provides a score, feedback, and improvement suggestions through a user-friendly **Streamlit web interface**.

---

## Features

1. Resume parsing and text extraction
2. Intelligent matching with job descriptions using Gemini 1.5 Pro
3. Compatibility scoring and strengths/weaknesses analysis
4. Feedback summary for resume optimization
5. Streamlit-based interactive dashboard

---

## TechStack

1. `Gemini 1.5 Pro` (Google Generative AI)
2. LangChain for LLM orchestration
3. Streamlit for UI
4. PyPDF2
5. Python (3.11)

---

## Workflow

### 1. Create Virtual Environment

```bash
python 3.11 -m venv 
# Activate the environment
source activate venv
``` 

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Save API Keys Securely

Create a .env file in the project root

### 4. Prompt Template

```bash
prompt_template = """
Job Description: {job_description}
Resume: {resume_text}
JD Match: {jd_match}%

As an ATS scanner and a Technical HR Manager, please provide an analysis of the resume based on the job description with the following details:
- JD Match: {jd_match}%
- Experience: [years]
- Skills Missing: [skills missing keywords]
- Overall Summary: [brief summary]
- Position Match: {position_match}
"""
```

### 5. Ingest Job Description & Resume (PDFs)

```bash
def input_pdf_text(uploaded_file):
    reader = pdf.PdfReader(uploaded_file)
    text = ""
    for page in reader.pages:
        text += page.extract_text() or ""
    return text
```

### 6. Extract Skills

Use CountVectorizer (for JD) and regex (for Resume):
```bash
def extract_skills(text):
    return set(re.findall(r'\b\w+\b', text.lower()))

def calculate_jd_match(job_description, resume):
    # Vectorizer for job description
    jd_vectorizer = CountVectorizer(stop_words='english', ngram_range=(1, 2), max_features=50)
    jd_vectorized = jd_vectorizer.fit_transform([job_description])
    jd_skills = set(jd_vectorizer.get_feature_names_out())

    # Vectorizer for resume
    resume_skills = extract_skills(resume)

    # Calculate match percentage
    match_percentage = len(jd_skills.intersection(resume_skills)) / len(jd_skills) * 100 if jd_skills else 0
    return round(match_percentage, 2)
```

### 8. Streamlit UI

```bash
streamlit run app.py
```

### 9. Upload to GitHub

```bash
git init
git remote add origin https://github.com/daleyprabhakar/GenAI_app_ATS_Gemini_pro.git
git add .
git commit -m "First Commit"
git push origin main
```
---

## Installation

1. Clone the repo:
   ```bash
   git clone https://github.com/daleyprabhakar/GenAI_app_ATS_Gemini_pro.git
   cd GenAI_app_ATS_Gemini_pro
   '''

## Using as a Streamlit App

1. **Login to [Streamlit Cloud](https://streamlit.io)** using your GitHub credentials.
2. Click on **"Create app"**.
3. Choose **"Create a public app from GitHub repo"**.
4. Enter your GitHub repository URL (e.g., `https://github.com/yourusername/ats-tracker`).
5. Click on **"Advanced Settings"** → **"Secrets"**, and add your secret keys:

    ```env
    GOOGLE_API_KEY=your_gemini_api_key
    ```

6. In the same screen, select:
    - **Branch:** `main`
    - **Main file path:** `ATS.py`
    - **Python version:** `3.11` (or as per your virtual environment)

7. Click **"Deploy"**. Once deployed, your app will be live. You can now upload resumes and job descriptions directly in the browser.
