# AI-Powered CV Screening App

A simple Streamlit application that compares uploaded CVs with job-related criteria using **Google Gemini 2.5 Flash**.

## Features

- Enter required education, minimum experience, relevant experience, preferred qualifications, and other job-related preferences.
- Upload multiple PDF, DOCX, and legacy DOC CVs.
- Extract CV text and ask Gemini to compare it with the criteria.
- Display candidate name, education match, total experience, relevant-experience match, preference match, and overall required-criteria result.
- Show short evidence excerpts and explanations.
- Export the summary table as CSV.
- Missing information is shown as **Not Found**, not guessed.

## Important notes

- This is a decision-support demo, not an automated hiring decision system. A qualified human must review the CV and the evidence before making any decision.
- Use only job-related criteria. Do not assess protected or personal characteristics such as age, gender, religion, ethnicity, disability, marital status, or information inferred from a photograph.
- An overall **No** can mean required information was not found or could not be verified. It does not necessarily mean the candidate lacks the qualification.
- PDF text is extracted with PyMuPDF. Image-only/scanned PDFs may have no extractable text and will be reported as unreadable. Run OCR or obtain a text-based PDF before uploading.
- DOCX files are read with python-docx. Legacy `.doc` files require the `antiword` system command; if it is unavailable, save the file as `.docx` or PDF.
- CV content is sent to Google's Gemini API. Obtain appropriate permission and follow applicable privacy, data-protection, and employment laws. Do not upload sensitive CVs to a public demo.
- AI outputs can be inaccurate. Check the evidence and original CV. The app does not store CVs in a database.

## 1. Project files

```text
cv_screening_app/
├── app.py
├── requirements.txt
└── README.md
```

## 2. Create a Gemini API key

1. Open [Google AI Studio](https://aistudio.google.com/).
2. Create an API key.
3. Keep the key private. Do not paste it into `app.py` or commit it to GitHub.

The app uses the model name `gemini-2.5-flash`.

## 3. Run locally

Use Python 3.10 or newer.

```bash
cd cv_screening_app
python -m venv .venv
```

Activate the environment:

**Windows PowerShell**
```powershell
.venv\Scripts\Activate.ps1
```

**macOS/Linux**
```bash
source .venv/bin/activate
```

Install packages:

```bash
pip install -r requirements.txt
```

Create a folder named `.streamlit`, then create `.streamlit/secrets.toml` inside it:

```toml
GEMINI_API_KEY = "paste-your-key-here"
```

Run the app:

```bash
streamlit run app.py
```

Do not upload `.streamlit/secrets.toml` to GitHub.

## 4. Push the project to GitHub

1. Sign in to GitHub and create a new repository, for example `ai-cv-screening-app`.
2. Open a terminal in the project folder.
3. Create a `.gitignore` file containing:

```gitignore
.venv/
__pycache__/
*.pyc
.streamlit/secrets.toml
.env
```

4. Run:

```bash
git init
git add app.py README.md requirements.txt .gitignore
git commit -m "Create AI CV screening app"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/ai-cv-screening-app.git
git push -u origin main
```

Replace `YOUR-USERNAME` with your GitHub username. Never commit API keys or real CV files.

## 5. Deploy on Streamlit Community Cloud

1. Push the project to GitHub.
2. Open [Streamlit Community Cloud](https://share.streamlit.io/) and sign in with GitHub.
3. Select **Create app**.
4. Choose your repository, branch (`main`), and main file path (`app.py`).
5. Open **Advanced settings** → **Secrets** and add:

```toml
GEMINI_API_KEY = "paste-your-key-here"
```

6. Click **Deploy**. Streamlit will install packages from `requirements.txt`.
7. When deployment finishes, open the app URL.

See the official [deployment guide](https://docs.streamlit.io/deploy/streamlit-community-cloud/deploy-your-app) and [secrets guide](https://docs.streamlit.io/deploy/streamlit-community-cloud/deploy-your-app/secrets-management).

## 6. Basic testing checklist

- Try a text-based PDF with a clearly stated name, degree, and work dates.
- Try a DOCX with the same information.
- Try a CV with no name or no employment dates; the relevant missing fields should show `Not Found`.
- Try a CV that does not meet the minimum experience; it should not be marked as an overall match.
- Try a scanned/image-only PDF; it may be reported as having no readable text.
- Check every AI result against the original CV before using it.

## Troubleshooting

- **Missing API key:** check `.streamlit/secrets.toml` locally or the Streamlit app's Secrets settings after deployment.
- **Quota / API errors:** check your Google AI Studio project, API key, model availability, and usage limits.
- **Unreadable PDF:** use a text-based PDF or OCR it before upload.
- **Legacy DOC not supported:** convert it to DOCX or PDF, or ensure `antiword` is installed on the host.
- **Dependency issue:** use Python 3.10+ and reinstall with `pip install -r requirements.txt`.
