# SidharthaErroju-Yellow-Bank-Agent

# Yellow Bank Agent 🤖

## 📌 Overview

This project is a Gen AI Banking Agent built using Yellow.ai platform.
It helps users securely view their loan details through authentication and API workflows.

---

## ⚙️ Features

* User Authentication (Phone + DOB + OTP)
* Loan Account Selection via Dynamic Cards
* Loan Details Display
* Token Optimization using Projection
* Edge Case Handling
* CSAT Feedback Flow

---

## 🔗 Mock APIs

* OTP API → Returns mock OTP
* Loan Accounts API → Returns multiple loan accounts (heavy JSON)
* Loan Details API → Returns selected loan details

---

## ⚡ Token Optimization (Projection)

Only required fields are sent to LLM:

* loan_account_id
* loan_type
* tenure

---

## 🔄 Flow Summary

1. User requests loan details
2. Collect phone & DOB
3. Generate OTP
4. Verify OTP
5. Show loan accounts
6. User selects account
7. Show loan details
8. Collect feedback

---

## 📂 Folder Structure

/mock-apis → API responses
/docs → Flow + System Prompt
/screenshots → UI (optional)

---

## 👤 Author

Sidhartha Erroju

📦 2. mock-apis FILES
➤ otp.json
{
  "status": "success",
  "data": {
    "otp": "1234"
  }
}

➤ loanAccounts.json (HEAVY JSON)
{
  "status": "success",
  "accounts": [
    {
      "loan_account_id": "LN001",
      "loan_type": "Home Loan",
      "tenure": "20 years",
      "interest_rate": 8.5,
      "principal_amount": 2500000,
      "emi": 21000,
      "branch_code": "HYD001",
      "risk": "low",
      "audit_flag": false,
      "currency": "INR"
    },
    {
      "loan_account_id": "LN002",
      "loan_type": "Car Loan",
      "tenure": "5 years",
      "interest_rate": 9.2,
      "principal_amount": 800000,
      "emi": 15000,
      "branch_code": "BLR002",
      "risk": "medium",
      "audit_flag": true,
      "currency": "INR"
  }
]
}


➤ loanDetails.json
{
  "status": "success",
  "data": {
    "tenure": "20 years",
    "interest_rate": 8.5,
    "principal_pending": 1800000,
    "interest_pending": 250000,
    "nominee": "Ravi Kumar"
  }
}


🧠 3. docs/system-prompt.txt
You are a Banking Assistant for Yellow Bank.

Rules:
- Only communicate in English.
- Authenticate user using Phone Number and DOB.
- Always trigger OTP verification before showing any data.
- Do not skip steps.
- Do not hallucinate data.
- If user changes number, restart authentication.
- Show loan accounts using only required fields.
- Always ask user to select an account before showing details.
- After showing details, ask for feedback.

Tone:
- Professional
- Helpful
- Secure
  
🔄 4. docs/flow-steps.md
# Flow Steps

1. User asks for loan details
2. Ask for phone number
3. Ask for DOB
4. Trigger OTP API
5. Ask OTP
6. Validate OTP
7. Call Loan Accounts API
8. Apply Projection
9. Show DRM cards
10. User selects account
11. Call Loan Details API
12. Show details
13. Redirect to CSAT

⚡ 5. docs/projection-logic.md
# Projection Logic (Token Optimization)

## Problem
Loan API returns heavy JSON with unnecessary fields.

## Solution
Filter only required fields before sending to LLM.

## Code
```javascript
var filteredAccounts = raw_accounts.map(acc => {
  return {
    loan_account_id: acc.loan_account_id,
    loan_type: acc.loan_type,
    tenure: acc.tenure
  };
});
Benefits
Reduces tokens
Improves performance
Avoids hallucination

---

# ⚠️ 6. docs/edge-cases.md

```md
# Edge Cases

## 1. Invalid OTP
- Show error
- Ask again

## 2. API Failure
- Show retry message

## 3. User Changes Number
- Clear phone & DOB
- Restart flow

## 4. Non-English Input
- Respond: "I can only assist in English."
