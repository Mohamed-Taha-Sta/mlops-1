# DevOps Assignment Report

## Overview
This report documents the implementation of a CI/CD pipeline for a Python ML application using GitHub Actions, Docker, and automated testing.

---

## Task 1: Prepare the ML Project

**What I did:**
- Forked the repository from the provided GitHub link
- Verified the repository structure and confirmed `requirements.txt` exists
- Cloned the repo locally: `git clone <your-fork-url>`

**Screenshot:** *(Insert screenshot of forked repo)*

---

## Task 2: Run the App Locally

**What I did:**
```bash
# Created virtual environment
python -m venv .venv

# Activated environment (Linux/Mac)
source .venv/bin/activate

# Installed dependencies
pip install -r requirements.txt

# Trained the model
python src/train.py

# Ran the app
python src/app.py
```

**Results:**
- Model trained successfully and saved to `models/` directory
- Flask app started on `http://localhost:5000`
- Tested `/predict` endpoint using curl or Postman

**Testing endpoints:**
```bash
curl -X POST http://localhost:5000/predict \
  -H "Content-Type: application/json" \
  -d '{"features": [5.1, 3.5, 1.4, 0.2]}'
```

**Screenshot:** *(Insert screenshot of running app and terminal output)*

---

## Task 3: Write Unit Tests

**What I did:**
- Created `tests/` directory
- Added `tests/test_model.py` with 3+ tests:
  - `test_model_training()` - verifies model can train
  - `test_prediction_format()` - checks prediction output format
  - `test_model_accuracy()` - validates model performs above baseline

**Running tests:**
```bash
pytest tests/ -v
```

**Results:** All tests passed

**Screenshot:** *(Insert screenshot of pytest output showing passed tests)*

---

## Task 4: Linting & Formatting

**What I did:**
- Installed flake8: `pip install flake8`
- Created `.flake8` config file with settings:
  - max-line-length = 120
  - exclude = .venv, __pycache__
- Ran linting: `flake8 src/ tests/`
- Fixed all linting errors

**Screenshot:** *(Insert screenshot of flake8 passing)*

---

## Task 5: GitHub Actions CI Workflow

**What I did:**
- Created `.github/workflows/ci.yml`
- Workflow triggers on: push and pull_request
- Pipeline steps:
  1. Checkout code
  2. Setup Python 3.9
  3. Install dependencies
  4. Run flake8 linting
  5. Run pytest tests
  6. Build Docker image
  7. Upload image as artifact

**Key decisions:**
- Used `actions/setup-python@v4` for Python setup
- Used `docker/build-push-action@v5` for Docker build
- Configured test results as artifacts for debugging

**Screenshot:** *(Insert screenshot of successful GitHub Actions run)*

---

## Task 6: Containerise the App

**What I did:**
- Created `Dockerfile`:
  - Base image: `python:3.9-slim`
  - Copied requirements and source code
  - Exposed port 5000
  - Set CMD to run Flask app

**Building and running:**
```bash
# Build image
docker build -t ml-app:latest .

# Run training in container
docker run ml-app:latest python src/train.py

# Run app in container
docker run -p 5000:5000 ml-app:latest
```

**Screenshot:** *(Insert screenshots of Docker build and running container)*

---

## CI/CD Pipeline Behavior

**On Push/PR:**
1. Code is checked out
2. Linting runs - fails if code style issues exist
3. Tests run - fails if any test fails
4. Docker image builds - fails if Dockerfile has errors
5. Artifacts uploaded - image available for download

**Benefits:**
- Catches bugs before merge
- Ensures code quality standards
- Automated testing saves time
- Reproducible builds via Docker

---

## How to Run Locally

### Without Docker:
```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python src/train.py
python src/app.py
```

### With Docker:
```bash
docker build -t ml-app .
docker run -p 5000:5000 ml-app
```

---

## Conclusion

Successfully implemented a complete CI/CD pipeline with automated testing, linting, and containerization. The pipeline ensures code quality and enables reliable deployments.