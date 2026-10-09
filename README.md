# English Handwriting Evaluation System

A small Flask application for B.Tech project study. Gemini performs two separate vision tasks: OCR reads the text, and evaluation scores six visible handwriting characteristics. Python calculates the final score and grade.

## Setup

1. Install Python 3.10 or newer and MySQL.
2. Create a virtual environment:

   ```powershell
   python -m venv .venv
   .\.venv\Scripts\Activate.ps1
   pip install -r requirements.txt
   ```

3. Copy `.env.example` to `.env` and fill in the MySQL and Gemini values. `load_dotenv()` loads these values into the process environment, and `os.getenv("GEMINI_API_KEY")` reads the key without hardcoding it.
4. Start the app. On startup, it connects to MySQL and creates the database and tables if they do not exist:

   ```powershell
   python app.py
   ```

Open `http://127.0.0.1:5000`.

## Flow for project review

Student login -> upload image -> `app.py:upload()` -> `image_processing.py:process_image()` -> `gemini.py:extract_text()` -> `gemini.py:evaluate_handwriting()` -> `app.py:calculate_score()` -> `app.py:calculate_grade()` -> `database.py:save_evaluation()` -> result page.

OCR answers “What did the student write?” Evaluation answers “How well is the handwriting written?” They are separate Gemini calls using the same configured vision model. Gemini does not calculate the final grade.

## Common errors

- A database error means MySQL is not running or the `.env` connection values are incorrect.
- A missing API-key error means `GEMINI_API_KEY` is absent from `.env`.
- An invalid evaluation response means Gemini did not provide the six required JSON scores; the app reports the error instead of inventing values.
