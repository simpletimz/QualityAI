# QualityAI - Intelligent Test Automation Suite combining SDET and AI/ML

# QualityAI — Intelligent Test Automation Suite

## Author
**Moses Opaleye** — SDET Engineer and AI/ML Developer  
GitHub: [@SimpleTimz](https://github.com/SimpleTimz)


![Python](https://img.shields.io/badge/Python-3.12-blue)
![Selenium](https://img.shields.io/badge/Selenium-4.44-green)
![pytest](https://img.shields.io/badge/pytest-9.0.3-orange)
![MLflow](https://img.shields.io/badge/MLflow-3.12-red)
![License](https://img.shields.io/badge/License-MIT-yellow)

## Overview
QualityAI is a production-grade intelligent test automation suite that combines SDET engineering with AI/ML capabilities. It automates web application testing, generates test cases using local LLMs, analyzes test failures intelligently, and provides actionable insights — all running completely offline.

## Problem Statement
Manual software testing is slow, inconsistent, and cannot scale with modern development speed. QA teams need automation that not only finds bugs faster but uses artificial intelligence to predict failure points, generate test cases automatically, and analyze results intelligently without sending sensitive data to external services.

## Features
- Automated UI testing using Selenium and pytest
- Automated API testing using Requests
- AI-powered test case generation using Ollama phi3
- Intelligent bug analysis and pattern recognition
- Local RAG pipeline for documentation querying
- CI/CD pipeline integration via Jenkins
- Professional test reporting via Allure
- Complete offline operation — no external AI APIs needed

## Tech Stack
| Layer | Technology |
|---|---|
| Language | Python 3.12 |
| UI Testing | Selenium 4.44 |
| Test Framework | pytest 9.0.3 |
| API Testing | Requests |
| AI Engine | Ollama phi3 (local LLM) |
| ML Framework | scikit-learn, MLflow |
| Reporting | Allure |
| CI/CD | Jenkins |
| Database | MySQL, PostgreSQL |
| Containerization | Docker |

## Project Structure
QualityAI/
├── ai_engine/
│   ├── test_generator/     → AI generates test cases
│   ├── bug_analyzer/       → AI analyzes failures
│   └── RAG_pipeline/       → document Q&A offline
├── data/                   → test data and SQL scripts
├── docs/                   → all documentation
├── jenkins/                → CI/CD pipeline config
├── reports/                → generated test reports
├── tests/
│   ├── ui_tests/           → Selenium browser tests
│   ├── api_tests/          → API endpoint tests
│   ├── performance_tests/  → load and stress tests
│   └── ai/                 → AI powered test cases
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt

## Installation
```bash
# Clone the repository
git clone https://github.com/SimpleTimz/QualityAI.git
cd QualityAI

# Create and activate virtual environment
python3 -m venv myenv
source myenv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

## Running Tests
```bash
# Run all tests
pytest tests/

# Run specific test category
pytest tests/ui_tests/
pytest tests/api_tests/

# Run with Allure reporting
pytest tests/ --alluredir=reports/allure-results
allure serve reports/allure-results
```