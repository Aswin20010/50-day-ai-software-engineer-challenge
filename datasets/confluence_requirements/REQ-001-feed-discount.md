# REQ-001 — FEED Discount API Requirement

## API Name

FEED Discount Evaluation API

## Purpose

This requirement represents an enterprise API that evaluates whether a transaction is eligible for employee or new-account discounts.

The API uses account interrogation, employee profile lookup, discount rule lookup, exclusion validation, additional discount checks, and final discount calculation.

---

## Endpoint

```text
POST /api/v1/discount/evaluateTransactionDiscount
```

---

## Input Sources

The API receives:

1. Request headers
2. Request body
3. Account information from interrogateAccount
4. Discount rules from PostgreSQL tables
5. Exclusion rules from PostgreSQL tables
6. Additional discount rules from PostgreSQL tables

---

## Core Business Flow

1. Validate request headers.
2. Validate request body.
3. Identify account using interrogateAccount.
4. Check whether the account belongs to an employee.
5. If employee account:

   * Determine home-shop or cross-shop logic.
   * Retrieve employee discount profile.
   * Validate exclusions.
   * Apply base discount.
   * Apply additional discount if eligible.
6. If non-employee account:

   * Evaluate new-account discount rules.
   * Validate exclusions.
   * Apply new-account discount percentage.
   * Apply configured cap amount.
7. Return final discount response.

---

## Database Tables

The API uses existing PostgreSQL tables:

```text
Emp_Acct_Profile
Emp_Disc_Profile
Disc_By_Dept_Vnd_Class
Disc_Dept_Vnd_Class_Exclusions
Addntl_Disc_Events
Addntl_Disc_By_Dept_Vnd_Class
Addtnl_Disc_Dept_Vnd_Class_Exclusions
FEED_CONFIG
```

---

## External Services

The API depends on:

```text
FEED /interrogateAccount
```

The account details should be echoed back from the interrogateAccount response.

---

## Expected Response

The response should include:

```text
code
message
transactionDetails
account
discountDetails[]
```

Each discount detail should include:

```text
lineSeqNumber
division
dept
vendor
class
upc
itemPriceInBag
discountEligible
discountedAmount
discountPercentApplied
additionalDiscountApplied
addDiscountPercentApplied
discountCode
discountDesc
isItemExcluded
```

---

## Notes

This file is an initial placeholder for Day 4 dataset creation.

It will be refined later using the complete Confluence requirement and Node-RED source flow.
