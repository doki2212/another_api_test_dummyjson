### DummyJSON API Test Suite (Postman + Newman)
### This project simulates real-world API testing as a QA automation engineer, demonstrating CRUD validation, negative testing, data-driven testing (DDT), dynamic variable handling, and CI-ready Newman execution.

## Overview

This project contains an automated API test suite for the DummyJSON API (https://dummyjson.com).
It showcases a complete QA automation workflow using Postman for designing tests and Newman for command-line execution and HTML reporting.
Because DummyJSON is a mock API, it does not persist created data—making it ideal for safe CRUD testing and negative validation.
This suite includes:
- Multiple GET endpoint tests (pagination, filtering, search)
- Simulated CRUD workflow
- Negative tests for unsupported update/delete operations
- Data-driven testing with CSV using the Postman Collection Runner & Newman

## Tech Stack
- Postman
- Newman (CLI)
- Node.js
- JavaScript (Postman test scripts)
- CSV (for data-driven testing)
- JSON

## Features Tested
# GET Endpoints
- GET /products – retrieve all products
- GET /products?limit&skip – pagination
- GET /products/search?q=phone – search functionality
- GET /products/category/smartphones – category filtering
- GET /products/1 – retrieve existing product

# CRUD Operations (Simulated)
DummyJSON does not persist created records and does not truly support PUT/DELETE, so CRUD testing is implemented using positive + negative flow:

- Create (POST)
  POST /products/add
  Creates a temporary product and extracts the returned ID
- Retrieve (GET)
  For persistent ID = 1 → positive test
  For newly created ID → negative 404 (expected due to API behavior)
- Update (PUT)
  PUT /products/{id} returns 404 Not Found
  Negative test validates correct error response
- Delete (DELETE)
  DELETE /products/{id} also returns 404 Not Found
  Negative test validates error message handling

This demonstrates the ability to test API mock environments with expected failure conditions.

# Data-Driven Testing (DDT)

Using products.csv, multiple POST requests are executed with unique title/price values.
title,price
Keyboard,75
Monitor,200
Mouse,30
Headphones,90
The suite validates that DummyJSON returns the correct values for each CSV row.

# Assertions Covered

- Status codes (200, 201, 400, 404)
- JSON body parsing
- Array and length validation
- Basic schema validation (id, title, price, stock)
- Search keyword validation
- Negative testing for unsupported operations
- CSV-driven input validation
- Dynamic environment variable extraction (product_id)
- Chaining requests using extracted data

# Project Structure
dummyjson-api-testing/
│
├── DummyJSON-API-Testing-Suite.postman_collection.json
├── dummyjson-env.postman_environment.json
├── products.csv
├── newman-report.html
└── README.md

# Setup & Execution
- Install Newman
npm install -g newman newman-reporter-html

- Run the full suite with data-driven CSV testing
newman run "DummyJSON-API-Testing-Suite.postman_collection.json" -e "dummyjson-env.postman_environment.json" --iteration-data "products.csv" --reporters cli,html --reporter-html-export newman-report.html

This will:
Execute all tests
Run 4 iterations (one per CSV row)
Generate an HTML report (newman-report.html)

# Expected Outcomes

- All assertions should pass, including:
- GET endpoints return correct fields
- Search results match keyword
- Pagination returns correct limit
- CSV POST requests match title/price inputs
- Negative tests correctly return 404 Not Found for unsupported PUT/DELETE
- JSON validation tests pass
- HTML Newman report is generated successfully

# Deliverables

This repository includes:
- Postman Collection (.postman_collection.json)
- Postman Environment (.postman_environment.json)
- Data File (products.csv)
- Newman HTML Report (newman-report.html)
- README Documentation (README.md)
These files ensure full reproducibility of the test suite.

# References

- DummyJSON API Docs: https://dummyjson.com
- Newman Documentation: https://learning.postman.com/docs/collection-runs/using-newman-cli/
- Postman Documentation: https://learning.postman.com/

# Author

Chriso Christudhas
QA Engineer | API Testing | Test Automation
GitHub: https://github.com/doki2212

# License

This project is open-source and free to use.
