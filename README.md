# 📊 Statistical Data Analysis Automation with Python

An end-to-end Python automation tool for quantitative statistical analysis, designed as an alternative to manual SPSS workflows. The project automatically performs data cleaning, descriptive statistics, instrument testing, classical assumption testing, and multiple linear regression using CSV questionnaire datasets.

Built for researchers, students, and analysts who want to automate statistical analysis in Google Colab with a single Python script.

---

# 🚀 Features

- Automatic CSV dataset import
- Data cleaning and preprocessing
- Descriptive statistics
- Pearson validity testing
- Cronbach's Alpha reliability testing
- Multiple Linear Regression
- Residual Normality Test (Kolmogorov–Smirnov)
- Multicollinearity Test (Tolerance & VIF)
- Heteroscedasticity Test (Glejser)
- Partial Hypothesis Test (t-test)
- Simultaneous Hypothesis Test (F-test)
- Coefficient of Determination (R²)

---

# 📈 Analysis Workflow

```
CSV Questionnaire
        │
        ▼
Data Cleaning
        │
        ▼
Descriptive Statistics
        │
        ▼
Validity Test
        │
        ▼
Reliability Test
        │
        ▼
Multiple Linear Regression
        │
        ▼
Classical Assumption Tests
        │
        ▼
Hypothesis Testing
        │
        ▼
Statistical Report
```

---

# 🛠 Technologies

- Python
- Google Colab
- Pandas
- NumPy
- SciPy
- Statsmodels

---

# 📌 Main Components

## 1. Dataset Import

Users upload a questionnaire dataset in CSV format directly through Google Colab.

```python
uploaded = files.upload()
```

---

## 2. Data Cleaning

The pipeline standardizes column names, converts values to numeric format, and removes missing values.

```python
df_all = df[columns].apply(pd.to_numeric, errors="coerce").dropna()
```

---

## 3. Descriptive Statistics

Automatically calculates:

- Sample Size (N)
- Minimum
- Maximum
- Mean
- Standard Deviation

---

## 4. Validity Test

Evaluates questionnaire items using Pearson Product Moment correlation.

```python
stats.pearsonr(...)
```

---

## 5. Reliability Test

Measures questionnaire consistency using Cronbach's Alpha.

```python
cronbach_alpha(...)
```

---

## 6. Multiple Linear Regression

Builds a regression model using Statsmodels.

```python
model = sm.OLS(...)
```

---

## 7. Classical Assumption Tests

The project automatically performs:

- Normality Test
- Multicollinearity Test
- Heteroscedasticity Test

---

## 8. Hypothesis Testing

Generates:

- t-Test
- F-Test
- R²
- Adjusted R²

---

# 📂 Project Structure

```text
.
├── statistical_analysis.py
├── README.md
└── sample_dataset.csv
```

---

# ⚙ Installation

```bash
git clone https://github.com/AlAkbar44/spss-automation-python.git

cd spss-automation-python
```

Install dependencies:

```bash
pip install pandas numpy scipy statsmodels
```

---

# ▶ Usage

Run the script in Google Colab.

Upload your CSV questionnaire dataset when prompted.

The program automatically performs:

- Data Cleaning
- Descriptive Statistics
- Validity Test
- Reliability Test
- Regression Analysis
- Classical Assumption Tests
- Hypothesis Testing

---

# 📊 Output

The program generates:

- Descriptive Statistics
- Validity Results
- Reliability Results
- Regression Coefficients
- ANOVA Table
- Normality Test
- Multicollinearity Test
- Heteroscedasticity Test
- t-Test
- F-Test
- R² Summary

---

# 📚 References

The statistical implementation follows widely used quantitative research methodologies, including:

- Ghozali (2018)
- Hair et al. (2019)
- Sugiyono (2019)

---

# 👨‍💻 Author

**Al Akbar Himawan**

Junior Data Analyst | SQL | Python | Statistics | Data Analysis

- LinkedIn: https://www.linkedin.com/in/alakbarhimawan
- GitHub: https://github.com/AlAkbar44
- Email: himawanalakbar6@gmail.com

---
