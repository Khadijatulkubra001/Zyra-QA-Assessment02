# University Programs Data Quality Validation Framework

## Overview

This project implements a Data Quality Validation Framework for university and academic program datasets collected from scraped and AI-assisted education data pipelines.

The solution validates data quality, identifies data issues, generates reports, compares dataset versions, provides a visual dashboard, and includes automated testing and CI/CD quality gates.

The primary goal is to ensure datasets meet defined quality standards before being consumed by downstream applications, analytics platforms, or reporting systems.

---

## Features

### Data Quality Validation

The validation framework performs automated checks across:

* Required field completeness
* Schema and type validation
* Identifier format validation
* Domain validation
* Range validation
* Source reliability validation
* Data freshness validation
* Duplicate detection

### Reporting

Generates:

* CSV issue reports
* JSON summary reports
* Production readiness assessment
* Quality score calculations
* Remediation recommendations

### Dashboard

Generates an HTML dashboard containing:

* Total records processed
* Failed records count
* Severity breakdown
* Quality score
* Production readiness status
* Issue category summary
* Interactive charts

### Dataset Comparison

Compares two dataset versions to determine whether data quality has improved or degraded.

Comparison metrics include:

* Critical issue count
* High issue count
* Medium issue count
* Low issue count
* Failed record count
* Overall quality score

### Automated Testing

Includes unit tests covering:

* Missing required fields
* Invalid schema values
* Invalid domain values
* Invalid ranges
* Duplicate detection
* Source validation
* Freshness validation

### CI/CD Integration

Includes a GitHub Actions workflow that automatically:

* Installs dependencies
* Executes unit tests
* Runs data validation
* Enforces quality gates
* Fails builds when validation thresholds are exceeded

---

## Project Structure

```text
.

│── universities_programs_v1.csv
│── universities_programs_v2.csv
│
├── data_validation.py
├── comparison_data.py
├── dashboard.py
│
├── Unit_test.py
│
├── data_quality_issues.csv
├── data_quality_report.json
├── data_quality_dashboard.html
│
├── ci.yml
│
├── README.md
└── Quality Rules.pdf
```

---

## Technology Stack

* Python
* Pandas
* Pytest
* GitHub Actions
* HTML
* Chart.js

---

## Setup

### Prerequisites

* Python 3.10+
* pip

### Install Dependencies

```bash
pip install pandas pytest
```

---

## Running Data Validation

Execute:

```bash
python data_validation.py
```

Outputs:

```text
data_quality_issues.csv
data_quality_report.json
```

The validation process:

1. Loads the dataset
2. Executes all quality checks
3. Identifies issues
4. Calculates quality metrics
5. Generates reports

---

## Generating the Dashboard

Execute:

```bash
python dashboard.py
```

Output:

```text
data_quality_dashboard.html
```

Open the generated HTML file in a browser to review the dashboard.

Dashboard metrics include:

* Total records
* Failed records
* Critical issues
* High issues
* Medium issues
* Low issues
* Quality score
* Production readiness

---

## Running Unit Tests

Execute:

```bash
python -m pytest -v
```

Example output:

```text
============================= test session starts =============================

collected 11 items

11 passed
```

The tests verify:

* Completeness validation
* Schema validation
* Domain validation
* Range validation
* Duplicate detection
* Source validation

---

## Comparing Dataset Versions

Execute:

```bash
python compare_versions.py
```

Example output:

| Metric          | V1 | V2 |
| --------------- | -- | -- |
| Critical Issues | 6  | 2  |
| High Issues     | 12 | 5  |
| Medium Issues   | 9  | 4  |
| Quality Score   | 71 | 88 |

Interpretation:

* Lower issue counts indicate improvement
* Higher issue counts indicate degradation
* Higher quality scores indicate better overall dataset quality

---

## Validation Categories

### Completeness

Ensures required fields are populated.

Examples:

* Missing institution name
* Missing program ID
* Missing source URL

Severity: Critical

---

### Schema Validation

Ensures values conform to expected data types and formats.

Examples:

* Tuition is not numeric
* Invalid date format
* Invalid boolean value

Severity: High

---

### Identifier Validation

Ensures identifiers follow expected patterns.

Examples:

* Invalid record_id format
* Invalid institution_id format
* Invalid program_id format

Severity: High

---

### Domain Validation

Ensures values match approved controlled vocabularies.

Examples:

* Invalid degree level
* Invalid delivery mode
* Invalid currency

Severity: High

---

### Range Validation

Ensures numeric values fall within acceptable business ranges.

Examples:

* Negative tuition
* Invalid GPA
* Invalid application year

Severity: Critical / High / Medium

---

### Source Validation

Ensures records contain valid source URLs.

Examples:

* Missing source URL
* Invalid HTTPS URL

Severity: Critical

---

### Freshness Validation

Ensures data is recent.

Examples:

* Records older than 12 months

Severity: Medium

---

### Duplicate Detection

Identifies duplicate records using:

* record_id
* Natural key

Natural key:

```text
institution_id +
program_name +
degree_level +
city +
application_year
```

Severity:

* Critical (duplicate record_id)
* High (duplicate natural key)

---

## Severity Model

| Severity | Description                                              |
| -------- | -------------------------------------------------------- |
| Critical | Blocks production use and requires immediate remediation |
| High     | Significant data quality issue                           |
| Medium   | Review recommended before release                        |
| Low      | Minor issue with limited business impact                 |

---

## Production Readiness Criteria

### Ready

Conditions:

* No Critical issues
* No High issues

### Ship With Caveats

Conditions:

* No Critical issues
* High or Medium issues present

### Not Ready

Conditions:

* One or more Critical issues detected

---

## Quality Score

The framework calculates a weighted quality score based on issue severity.

Penalty weights:

| Severity | Weight |
| -------- | ------ |
| Critical | 10     |
| High     | 5      |
| Medium   | 2      |
| Low      | 1      |

Formula:

```text
Quality Score = 100 - Weighted Penalty
```

This provides a simple overall measure of dataset health.

---

## CI/CD Quality Gate

The project includes a GitHub Actions workflow:

```text
.github/workflows/ci.yml
```

The workflow automatically runs on:

* Push events
* Pull requests

Pipeline steps:

1. Checkout repository
2. Install dependencies
3. Run unit tests
4. Execute validation framework
5. Enforce quality thresholds

Example quality gate:

* Critical Issues = 0
* Unit Test Failures = 0

If thresholds are exceeded, the pipeline fails.

---

## Extending the Framework

Adding new validation rules requires four steps:

### 1. Create a Validation Method

```python
def validate_program_duration(self):
    ...
```

### 2. Register the Rule

Add the method to the `run()` function.

```python
self.validate_program_duration()
```

### 3. Add Unit Tests

Create test coverage for the new rule.

```python
def test_program_duration():
    ...
```

### 4. Update Documentation

Document:

* Rule purpose
* Expected values
* Severity level
* Example failures

This ensures validation logic, tests, reporting, and documentation remain aligned.

---

## Deliverables

This assessment includes:

* Data Quality Validation Framework
* CSV Issue Reporting
* JSON Summary Reporting
* HTML Dashboard
* Dataset Comparison Module
* Automated Unit Tests
* GitHub Actions CI/CD Pipeline
* Automated Quality Gates
* Production Readiness Assessment
* Data Quality Documentation

---

## Future Enhancements

Potential future improvements include:

* Historical quality trend analysis
* Rule configuration via YAML or JSON
* Database-backed reporting
* Data lineage tracking
* Dashboard filtering and drill-down capabilities
* Notification integration for validation failures
* Automated remediation recommendations
