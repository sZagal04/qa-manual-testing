# 📋 QA Manual Testing — Urban Grocers API (Postman)

Manual API test suite for the Urban Grocers platform, executed with **Postman** and documented in Excel. Tests the kit products and delivery services endpoints using boundary value analysis across multiple fare tiers.

---

## 🎯 What was tested

Two API endpoints were tested to validate business rules around product limits per kit and delivery fare tier boundaries.

**Endpoints covered:**
- `POST /api/v1/kits/:id/products` — Add products to a grocery kit
- `POST /order-and-go/v1/delivery` — Delivery service fare calculation

**Testing focus:**
- Boundary value analysis on product quantity and kit size limits
- Boundary value analysis on delivery fare thresholds ($3 tier and $5 tier)
- Negative scenarios: missing fields, wrong data types (strings, floats where integers expected)
- Happy path: valid inputs with all required fields
- Error handling: non-existent kit IDs, empty product lists

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| Postman | API request execution and manual test runs |
| Excel | Test case documentation and results tracking |
| Jira | Bug tracking and defect reporting (KAN board) |
| apiDoc | API documentation reference |

---

## 📁 Project Structure

```
qa-manual-testing/
│
├── test_cases_sprint4.xlsx   # 42 test cases with steps, expected vs actual results, Jira links
└── README.md
```

---

## 📊 Results Summary

| Metric | Value |
|--------|-------|
| Total test cases | 42 |
| Passed | 31 |
| Failed | 11 |
| Bugs reported in Jira | 11 |

---

## 🐛 Bugs Found

| Jira ID | Endpoint | Scenario | Expected | Actual |
|---------|----------|----------|----------|--------|
| KAN-4 | `/kits/:id/products` | Float value as product ID | 400 Bad Request | 500 Internal Server Error |
| KAN-5 | `/kits/:id/products` | Float value as quantity | 400 Bad Request | 500 Internal Server Error |
| KAN-6 | `/order-and-go/v1/delivery` | `deliveryTime` below $3 tier minimum | 400 Bad Request | 200 OK |
| KAN-7 | `/order-and-go/v1/delivery` | `deliveryTime` above $3 tier maximum | 400 Bad Request | 200 OK |
| KAN-8 | `/order-and-go/v1/delivery` | `productsCount` below $3 tier minimum | 400 Bad Request | 200 OK |
| KAN-10 | `/order-and-go/v1/delivery` | `productsWeight` below $3 tier minimum | 400 Bad Request | 200 OK |
| KAN-12 | `/order-and-go/v1/delivery` | `deliveryTime` below $5 tier minimum | 400 Bad Request | 200 OK |
| KAN-13 | `/order-and-go/v1/delivery` | `deliveryTime` above $5 tier maximum | 400 Bad Request | 200 OK |
| KAN-15 | `/order-and-go/v1/delivery` | `productsCount` above $5 tier maximum | 400 Bad Request | 200 OK |
| KAN-16 | `/order-and-go/v1/delivery` | `productsWeight` below $5 tier minimum | 400 Bad Request | 200 OK |
| KAN-17 | `/order-and-go/v1/delivery` | `productsWeight` above $5 tier maximum | 400 Bad Request | 200 OK |

Most failures follow a pattern: the API returns **200 OK** when out-of-range values should be rejected with **400 Bad Request**, indicating the delivery service boundary validation logic is not enforced server-side.

---

## 🧪 Test Design Techniques Applied

- **Boundary Value Analysis** — tested values just below, at, and just above each fare tier limit for `deliveryTime`, `productsCount`, and `productsWeight`
- **Equivalence Partitioning** — grouped valid/invalid input classes for product IDs (integers vs floats vs strings)
- **Negative Testing** — missing required fields, wrong data types, non-existent kit IDs

---

## 🔗 Related projects

- [UI Automation](https://github.com/sZagal04/qa-automation-urban-routes)
- [API Testing](https://github.com/sZagal04/qa-api-testing)
- [Database Testing](https://github.com/sZagal04/qa-database-testing)
