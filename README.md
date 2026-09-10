# 🛒 eCom API Automation Suite

[![API Testing](https://img.shields.io/badge/API%20Testing-Postman-orange?logo=postman)](https://www.postman.com/)
[![Newman](https://img.shields.io/badge/Newman-CLI%20Runner-blue?logo=newman)](https://github.com/postmanlabs/newman)
[![JavaScript](https://img.shields.io/badge/Test%20Scripts-JavaScript-yellow?logo=javascript)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Execution](https://img.shields.io/badge/Execution-602%20Assertions%20%7C%200%20Failures-success)](reports/ecom-api-report.html)
[![Target API](https://img.shields.io/badge/Target-DummyJSON%20E--commerce-lightgrey)](https://dummyjson.com/)

> A production-minded Postman and Newman API automation suite for validating authentication, product services, and shopping cart workflows against the DummyJSON e-commerce API.

## 📋 Overview

This project demonstrates an end-to-end API quality engineering approach using Postman, JavaScript assertions, environment-driven configuration, response contract validation, and Newman reporting.

The collection is organized around real API dependencies. Authentication runs first, generated credentials and tokens are captured, and downstream requests reuse runtime values such as `userId`, `productId`, and `cartId`. This makes the suite repeatable, maintainable, and suitable for local or CI execution.

## 🧪 Quality Engineering Highlights

- Reusable collection-level pre-request and test scripts
- Authentication and bearer-token chaining
- Response contract and JSON schema validation
- Positive and negative response validation
- Response-time service-level thresholds
- Runtime extraction of IDs and tokens
- Query parameter validation for pagination and sorting
- CRUD coverage for products and carts
- Newman CLI execution with HTML reporting
- Environment-based configuration without hardcoded runtime state

## 🔌 API Coverage

### 🔐 Authentication and Profile
- Login with access and refresh token validation
- Refresh token workflow
- Authenticated current-user profile request
- Token and user ID extraction for downstream requests
- Email, image URL, profile field, and schema assertions

### 🛍️ Products

- Retrieve all products
- Retrieve a single product
- Search products
- Pagination using `limit` and `skip`
- Sort products using `sortBy` and `order`
- Retrieve product categories and category list
- Search products by category and filters
- Add, update, and delete products
- Product field, URL, pagination, and schema assertions

### 🛒 Carts

- Retrieve all carts
- Retrieve a single cart
- Retrieve carts for a user
- Cart pagination and sorting
- Add, update, and delete carts
- Cart collection contract validation
- Chained user, product, and cart identifiers

## 🔄 Test Execution Flow

```text
Login
  -> capture accessToken, refreshToken, userId
  -> Refresh Token
  -> Get User Profile
  -> Get Products
  -> capture productId
  -> Product search, filters, categories, CRUD
  -> Get Carts
  -> capture cartId
  -> Cart search, filters, sorting, CRUD
```

Collection-level scripts provide shared quality gates. Request-level scripts add endpoint-specific assertions where the business response requires deeper validation.

## ✅ Assertion Strategy

The suite validates API behavior at multiple levels:

| Validation layer | Examples |
|---|---|
| Request readiness | Base URL, resolved URL, HTTP method, JSON body, required variables |
| Protocol behavior | Successful status codes, valid error status range, JSON response format |
| Contract validation | Required fields, data types, object and array structure, JSON schema |
| Functional behavior | Token generation, profile fields, product and cart identifiers, CRUD responses |
| Data integrity | Pagination consistency, non-empty values, chained IDs, response/request relationships |
| Performance baseline | Response-time threshold for every request |
| Negative behavior | Error or message content for unsuccessful responses |

## 🔗 Chaining and Runtime Variables

The collection stores values from successful responses and reuses them in dependent requests:

| Variable | Purpose |
|---|---|
| `base_url` | API host configuration |
| `Token` | Bearer token used by the profile request |
| `accessToken` | Runtime access token from authentication |
| `refreshToken` | Token used by the refresh workflow |
| `userId` | Authenticated user and cart lookup dependency |
| `productId` | Product lookup, update, and delete dependency |
| `createdProductId` | ID captured after product creation |
| `cartId` | Cart update and delete dependency |

Runtime tokens are generated during execution and should not be committed as permanent secrets.

## 📁 Project Structure

```text
.
|-- eCom_Collection.postman_collection.json
|   `-- Requests, scripts, assertions, and collection-level quality gates
|-- eCom_Environment.postman_environment.json
|   `-- Base URL, demo credentials, and runtime variables
|-- data/
|   `-- ecom-query-iterations.json
|       `-- Data-driven search, pagination, category, and sort inputs
|-- reports/
|   `-- ecom-api-report.html
|       `-- Newman HTML execution report
|-- .gitignore
`-- README.md
```

## ⚙️ Prerequisites

- Node.js and npm
- Newman
- `newman-reporter-htmlextra`
- Internet access to `https://dummyjson.com`

🚀 Run with Postman

1. Import `eCom_Collection.postman_collection.json`.
2. Import `eCom_Environment.postman_environment.json`.
3. Select the `eCom API - QA` environment.
4. Confirm that `base_url` points to the target API.
5. Run the collection in the Postman Collection Runner.

🧰 Run with Newman

Install Newman and the HTML reporter:

```powershell
npm install -g newman newman-reporter-htmlextra
```

Run the full collection and generate the report:

```powershell
newman run .\eCom_Collection.postman_collection.json `
  -e .\eCom_Environment.postman_environment.json `
  --reporters cli,htmlextra `
  --reporter-htmlextra-export .\reports\ecom-api-report.html
```

Run the data-driven suite with multiple external test-data rows:

```powershell
newman run .\eCom_Collection.postman_collection.json `
  -e .\eCom_Environment.postman_environment.json `
  -d .\data\ecom-query-iterations.json `
  --reporters cli,htmlextra `
  --reporter-htmlextra-export .\reports\ecom-api-report.html
```

If PowerShell blocks the `newman.ps1` shim, use the Windows command shim:

```powershell
newman.cmd run .\eCom_Collection.postman_collection.json `
  -e .\eCom_Environment.postman_environment.json `
  --reporters cli,htmlextra `
  --reporter-htmlextra-export .\reports\ecom-api-report.html
```

## 📊 Execution Evidence

The included Newman run completed successfully:

| Metric | Result |
|---|---:|
| Iterations | 2 |
| Requests executed | 56 |
| Assertions | 602 |
| Failed assertions | 0 |
| Test scripts | 112 |
| Pre-request scripts | 62 |
| Average response time | 224 ms |
| Total duration | 18.7 seconds |

Detailed report: [Open the Newman HTML report](https://htmlpreview.github.io/?https://raw.githubusercontent.com/tawsif5001/ecommerce-api-automation-postman-newman/main/reports/ecom-api-report.html)

> Results can vary because the suite uses a public API and execution time depends on network and server conditions.

## 📈 Data-Driven, Iteration, and Performance Roadmap

The suite covers negative tests, boundary tests, business-rule validation, endpoint-specific assertions, and external JSON-driven Newman iterations. Recommended next steps for continued automation maturity are:

- Add malformed payload and missing-field scenarios for mutation endpoints
- Run controlled repeated iterations with `--iteration-count` alongside the data file
- Track response-time trends and define endpoint-specific SLAs
- Use k6, JMeter, or Artillery for realistic concurrent load testing
- Publish Newman reports as CI artifacts through GitHub Actions or Jenkins
- Add pipeline quality gates for assertion failures and latency thresholds

## 🔒 Security and Maintenance Notes

- The included credentials are demo credentials for a public test API.
- Never commit production credentials, API keys, or live tokens.
- Keep environment-specific values in environment files or CI secrets.
- Treat generated IDs and tokens as disposable runtime state.
- Re-run the collection when the public API contract changes.

## 💼 Project Context

This project focuses on API quality engineering using Postman, JavaScript, and Newman, with coverage across authentication, API contract validation, dynamic data management, request chaining, CRUD workflows, negative testing, performance baselining, command-line execution, and test reporting.

## 👨‍💻 Author

**Tawsif Ahmed**  
SQA Engineer | API Automation | Postman & Newman
