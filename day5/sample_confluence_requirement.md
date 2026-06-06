# Day 5 — Phase 2: Sample Confluence Requirement Document

## Phase Objective

The objective of Phase 2 is to create a sample enterprise-style Confluence requirement document.

This document will act as the raw input for the requirement extraction layer.

The goal is to simulate how real enterprise API requirements are usually written before they are converted into structured JSON, Node-RED flows, Excel specifications, or generated service code.

---

## Research Project

**Automating Enterprise API Development using AI-Driven Requirement and Workflow Synthesis**

---

## Phase Context

In Day 5, the project begins the first real implementation foundation:

```text
Confluence Requirement Document
        ↓
Structured Requirement JSON
```

The sample requirement created in this phase will be used in the next phases to define:

* Requirement extraction schema
* Extracted requirement JSON
* Extraction rules
* Traceability mapping
* Future Node-RED generation input
* Future Excel generation input

---

# Sample Confluence Requirement Document

## API Name

Evaluate Transaction Discount API

---

## API Description

This API evaluates whether a customer transaction is eligible for an employee discount or a new account discount.

The API receives customer account information, transaction context, and item-level purchase details. It applies validation, account interrogation, database lookup, exclusion, and discount calculation rules to determine the final discount result.

The API must first validate the incoming request, then interrogate the customer account, determine the correct discount path, apply eligible rules, calculate item-level discounts, and return a structured response.

---

## Endpoint Details

| Field         | Value                                        |
| ------------- | -------------------------------------------- |
| HTTP Method   | POST                                         |
| Endpoint Path | /api/v1/discount/evaluateTransactionDiscount |
| Content-Type  | application/json                             |

---

## Required Headers

| Header Name               | Required | Description                                                         |
| ------------------------- | -------- | ------------------------------------------------------------------- |
| requestorInfo-clientId    | Yes      | Client application identifier                                       |
| requestorInfo-subclientId | Yes      | Sub-client application identifier                                   |
| requestorInfo-traceId     | No       | Optional trace identifier for request tracking                      |
| requestorinfo-version     | No       | Optional requestor version                                          |
| x-macys-apikey            | Yes      | API key used for authentication                                     |
| x-hmac-secret             | Yes      | Secret used to generate HMAC signature for downstream service calls |

---

## Request Body Example

```json
{
  "account": {
    "encryptInfo": {
      "encryptedData": "string",
      "keyType": "RSA",
      "keyName": "string",
      "keyVersion": "string",
      "division": 71,
      "storeNbr": 123,
      "aesType": "string"
    },
    "entryMode": "KEYED"
  },
  "transaction": {
    "division": 71,
    "storeNbr": 123,
    "items": [
      {
        "upc": "123456789012",
        "department": 100,
        "vendor": 200,
        "classNbr": 300,
        "price": 100.00,
        "quantity": 1
      }
    ]
  }
}
```

---

## Field-Level Requirements

| Field Path                        | Type    | Required | Description                 |
| --------------------------------- | ------- | -------- | --------------------------- |
| account.encryptInfo.encryptedData | string  | Yes      | Encrypted account data      |
| account.encryptInfo.keyType       | string  | Yes      | Encryption key type         |
| account.encryptInfo.keyName       | string  | Yes      | Encryption key name         |
| account.encryptInfo.keyVersion    | string  | Yes      | Encryption key version      |
| account.encryptInfo.division      | integer | Yes      | Account division number     |
| account.encryptInfo.storeNbr      | integer | Yes      | Account store number        |
| account.encryptInfo.aesType       | string  | No       | Optional AES type           |
| account.entryMode                 | string  | Yes      | Account entry mode          |
| transaction.division              | integer | Yes      | Transaction division number |
| transaction.storeNbr              | integer | Yes      | Transaction store number    |
| transaction.items[].upc           | string  | Yes      | Item UPC                    |
| transaction.items[].department    | integer | Yes      | Item department number      |
| transaction.items[].vendor        | integer | Yes      | Item vendor number          |
| transaction.items[].classNbr      | integer | Yes      | Item class number           |
| transaction.items[].price         | number  | Yes      | Item price                  |
| transaction.items[].quantity      | integer | Yes      | Item quantity               |

---

## Enum Requirements

### Allowed `keyType` Values

The field `account.encryptInfo.keyType` must match one of the following values:

* AES
* RSA
* IRN
* AES_PAN
* TOKEN
* MAN
* AES_RSA_HYBRID
* CLEAR_TEXT
* EZ_ENCRYPTION
* ECV1
* ECV2

---

### Allowed `entryMode` Values

The field `account.entryMode` must match one of the following values:

* KEYED
* BARCODE_SCAN
* ACCT_LOOKUP_WITHIN_TRANSACTION
* ACCT_LOOKUP_SHOPPING_PASS
* CONTACTLESS
* B_CONNECTED
* CLIENTELE
* DCR_LOOKUP
* MOBILE_WALLET_GOOGLE
* NEW_ACCT_TRANSACTION
* MOBILE_WALLET_ISIS
* NEW_ACCT_APPROVED
* MOBILE_WALLET_SOFTCARD
* NEW_ACCT_NOT_APPROVED
* PIN
* SWIPED
* TAP
* TOKENIZED

---

# Business Rules

## BR-001 — Validate Mandatory Headers

The API must validate all required headers before processing the request.

If any required header is missing, the API must return a `400 Bad Request` response.

Required headers include:

* requestorInfo-clientId
* requestorInfo-subclientId
* x-macys-apikey
* x-hmac-secret

Failure behavior:

```text
Return 400 Bad Request with the missing header name.
```

---

## BR-002 — Validate Request Body

The API must validate required request body fields before performing any downstream service call or database lookup.

Required request body fields include:

* account.encryptInfo.encryptedData
* account.encryptInfo.keyType
* account.encryptInfo.keyName
* account.encryptInfo.keyVersion
* account.encryptInfo.division
* account.encryptInfo.storeNbr
* account.entryMode
* transaction.division
* transaction.storeNbr
* transaction.items

Each transaction item must include:

* upc
* department
* vendor
* classNbr
* price
* quantity

Failure behavior:

```text
Return 400 Bad Request with the missing field name.
```

---

## BR-003 — Validate Enum Fields

The API must validate `account.encryptInfo.keyType` and `account.entryMode`.

If either value does not match the allowed enum list, the API must return a `400 Bad Request` response.

Failure behavior:

```text
Return 400 Bad Request with enum validation error.
```

---

## BR-004 — Build Interrogate Account Request

The API must build a downstream request for the Interrogate Account service.

The downstream request must include:

* Encrypted account data
* Key type
* Key name
* Key version
* Division
* Store number
* AES type, if available
* Entry mode

The API should derive PIID values internally from encrypted account data.

The client should not provide PIID values directly.

---

## BR-005 — Invoke Interrogate Account Service

The API must call the downstream Interrogate Account service to identify account information.

The downstream service determines whether the account is associated with:

* Employee account
* New account
* Non-discount eligible account

The API must pass requestor information headers when available.

The downstream service must be called before any discount eligibility path is finalized.

---

## BR-006 — Employee Discount Eligibility

If the account is identified as an employee account, the API must evaluate employee discount rules.

The API must check:

* Employee account profile
* Employee discount profile
* Department/vendor/class discount rules
* Department/vendor/class exclusion rules
* Additional discount events
* Additional department/vendor/class discount rules
* Additional exclusion rules

The employee discount path should be selected only when the account interrogation result identifies the customer as an employee.

---

## BR-007 — New Account Discount Eligibility

If the account is not identified as an employee account, the API must evaluate new account discount rules.

The API must check:

* New account eligibility period
* New account discount percentage
* New account discount cap amount
* Department/vendor/class exclusion rules

The new account discount path should only be selected when the account qualifies for new account discount rules.

---

## BR-008 — Calculate Discount

The API must calculate discount amount at the item level.

The discount calculation should use the following logic:

```text
discountAmount = itemPrice * discountPercent / 100
```

If additional discount percentage applies, the total discount percentage should include both base discount and additional discount.

Example:

```text
baseDiscountPercent = 20
additionalDiscountPercent = 5
totalDiscountPercent = 25
```

---

## BR-009 — Map Final Response

The API must return a final response containing:

* Response code
* Discount type
* Discount percentage applied
* Additional discount percentage, if applicable
* Item-level discount amount
* Account information returned from interrogation

The response must clearly indicate whether the discount type is:

* Employee
* NewAccount
* None

---

# External Services

## Interrogate Account Service

| Field        | Value                                                        |
| ------------ | ------------------------------------------------------------ |
| Service Name | Interrogate Account Service                                  |
| Method       | POST                                                         |
| URL          | /accountinterrogation/api/v1/accountinterrogation/piidLookup |
| Purpose      | Identifies account details and discount eligibility path     |

---

## Downstream Service Rules

The downstream Interrogate Account service must be called using HMAC authentication.

The HMAC signature must be generated using SHA-256 and encoded as Base64.

The generated signature must be passed using the following header format:

```text
HMAC: MAC <signature>
```

The downstream request must pass requestor information headers when available.

---

## Downstream Request Mapping

| Source Field                      | Target Field                          |
| --------------------------------- | ------------------------------------- |
| account.encryptInfo.encryptedData | elements[0].encryptInfo.encryptedData |
| account.encryptInfo.keyType       | elements[0].encryptInfo.keyType       |
| account.encryptInfo.keyName       | elements[0].encryptInfo.keyName       |
| account.encryptInfo.keyVersion    | elements[0].encryptInfo.keyVersion    |
| account.encryptInfo.division      | elements[0].encryptInfo.division      |
| account.encryptInfo.storeNbr      | elements[0].encryptInfo.storeNbr      |
| account.encryptInfo.aesType       | elements[0].encryptInfo.aesType       |
| account.entryMode                 | elements[0].entryMode                 |

---

## Downstream Response Mapping

| Downstream Field     | API Context Field            |
| -------------------- | ---------------------------- |
| employeeFlag         | account.employeeFlag         |
| internalAccount      | account.internalAccount      |
| internalAccountToken | account.internalAccountToken |
| internalAccountLast4 | account.internalAccountLast4 |
| lookupType           | account.lookupType           |
| rc                   | account.rc                   |

---

# Database Tables

| Table Name                            | Purpose                                                            |
| ------------------------------------- | ------------------------------------------------------------------ |
| Emp_Acct_Profile                      | Stores employee account profile information                        |
| Emp_Disc_Profile                      | Stores employee discount profile rules                             |
| Disc_By_Dept_Vnd_Class                | Stores base discount rules by department, vendor, and class        |
| Disc_Dept_Vnd_Class_Exclusions        | Stores exclusion rules for base discount                           |
| Addntl_Disc_Events                    | Stores additional discount event configuration                     |
| Addntl_Disc_By_Dept_Vnd_Class         | Stores additional discount rules                                   |
| Addtnl_Disc_Dept_Vnd_Class_Exclusions | Stores exclusion rules for additional discount                     |
| FEED_CONFIG                           | Stores configurable values such as cap amount and eligibility days |

---

# Success Response Example

```json
{
  "code": "SUCCESS",
  "discountType": "Employee",
  "discountPercentApplied": 20,
  "additionalDiscountPercentApplied": 5,
  "accountInfo": {
    "internalAccount": "string",
    "internalAccountToken": "string",
    "internalAccountLast4": "1234",
    "lookupType": "PIID",
    "rc": "0"
  },
  "items": [
    {
      "upc": "123456789012",
      "department": 100,
      "vendor": 200,
      "classNbr": 300,
      "price": 100.00,
      "quantity": 1,
      "discountAmount": 25.00
    }
  ]
}
```

---

# Error Response Example

```json
{
  "code": "BAD_REQUEST",
  "message": "Missing required header: requestorInfo-clientId"
}
```

---

# Extraction Notes

This requirement document intentionally includes multiple requirement sections that can be extracted into structured JSON.

The extraction layer should identify:

* API metadata
* Endpoint details
* Headers
* Request body fields
* Enum validations
* Business rules
* External service details
* Request mappings
* Response mappings
* Database table references
* Success response structure
* Error response structure

The extraction process must preserve enterprise field names exactly as written.

Examples:

```text
requestorInfo-clientId
account.encryptInfo.keyType
transaction.items[].classNbr
Disc_By_Dept_Vnd_Class
```

The extraction process must not rename fields into simplified names unless explicitly required by a mapping rule.

---

# Phase 2 Outcome

Phase 2 creates the raw requirement document that will be used as input for requirement extraction.

This document represents the kind of semi-structured enterprise API requirement that the research project aims to convert into structured machine-readable artifacts.

The next phase will define the structured requirement schema that this document should be converted into.

---

# Status

Phase 2 completed successfully.
