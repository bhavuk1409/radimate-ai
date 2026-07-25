# RadiMate AI — Radiology & Blood Report Analyzer

A Streamlit app that takes radiology reports (CT/MRI/X-ray/USG) and
blood/lab reports (CBC/LFT/KFT/etc.) — as images or PDFs — extracts the
text via OCR, analyzes everything together with an LLM, and generates a
downloadable, formatted PDF summary.

**Live:** https://radimate.streamlit.app

![python](https://img.shields.io/badge/python-3.x-3776ab) ![ui](https://img.shields.io/badge/UI-Streamlit-ff4b4b) ![ocr](https://img.shields.io/badge/OCR-Tesseract-4285f4) ![llm](https://img.shields.io/badge/LLM-Groq%20(Llama%203.3)-f55036)

> ⚠️ **This tool generates AI-assisted summaries of medical reports for
> informational purposes only. It does not diagnose, prescribe, or replace
> professional medical consultation.** Every generated report ends with
> that disclaimer, and the underlying prompt is explicitly restricted from
> giving diagnoses, prescriptions, or emergency/surgical guidance.

---

## What it does

1. **Upload** one or more radiology or blood/lab reports — images
   (PNG/JPG/BMP/TIFF) or PDFs, multiple files at once.
2. **OCR** — each file is preprocessed (grayscale, contrast, upscaling,
   noise filtering) and run through **Tesseract** to extract text. PDFs are
   converted page-by-page to images first.
3. **Combined AI analysis** — all extracted text is sent together to a
   Groq-hosted **Llama 3.3 70B** model, using a system prompt that:
   - Restricts analysis strictly to radiology and blood/lab report content
   - Politely declines if the upload isn't a valid medical report
   - Interprets radiology findings (normal vs. abnormal, severity)
   - Breaks down blood parameters (value, reference range, low/normal/high)
   - Always includes a plain-language **"Full Summary"** and a
     non-prescriptive **"Advice"** section
   - Never fabricates values, diagnoses definitively, or gives medication
     dosages
4. **PDF generation** — the AI's analysis is rendered into a downloadable,
   formatted PDF report.

---

## Repository layout

```
├── app.py                  # Full Streamlit app: upload, OCR, LLM analysis, PDF export
├── requirements.txt         # Python dependencies
├── packages.txt               # System packages (Tesseract, Poppler, etc.)
└── .devcontainer/               # Dev container config
```

---

## Quickstart

### Prerequisites

- Python 3.x
- System packages (see `packages.txt`): `tesseract-ocr`, `libtesseract-dev`,
  `libleptonica-dev`, `poppler-utils` — the last one (`poppler-utils`) is
  required for PDF-to-image conversion.
- A Groq API key

### Install

```bash
git clone https://github.com/bhavuk1409/my-ai-radiologist.git
cd my-ai-radiologist

# System dependencies (Debian/Ubuntu example)
sudo apt-get install -y $(cat packages.txt)

pip install -r requirements.txt
```

### Configure

Create a `.env` file:

```
GROQ_API_KEY=your_groq_api_key
```

### Run

```bash
streamlit run app.py
```

Opens at `http://localhost:8501`. Upload one or more reports, review the
extracted OCR text (expandable), read the generated analysis, and download
the PDF.

---

## Tech stack

- **Streamlit** — UI
- **Tesseract (`pytesseract`)** + **Pillow** — image preprocessing and OCR
- **`pdf2image` (Poppler)** — converts PDF pages to images for OCR
- **LangChain + `langchain-groq`** — prompt orchestration and LLM calls
- **Groq (Llama 3.3 70B)** — report analysis
- **FPDF** — generates the downloadable PDF report

---

## Notes

- `requirements.txt` includes some unused dependencies (`langgraph`,
  `langserve`, `fastapi`, `uvicorn`, `replicate`, `sse_starlette`,
  `opencv-python`, `reportlab`) not referenced in `app.py` — worth pruning
  if you want a leaner install.
- OCR accuracy depends heavily on input image quality; scanned reports
  with low resolution or heavy compression may extract poorly.

---

## License

Add your license here.
