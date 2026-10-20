# ⚖️ PARAKH

### AI-Assisted Legal Metrology Compliance & Inspection System

**PARAKH** is an AI-assisted compliance platform developed for **Smart India Hackathon 2026 – Problem Statement 26034**, aimed at digitizing the inspection of packaged commodities under the **Legal Metrology (Packaged Commodities) Rules, 2011**.

The platform allows an inspector to upload a packaged-product image, extract relevant declarations using **Gemini Vision**, validate those declarations against a **rule-based Legal Metrology compliance engine**, identify violations, and generate a structured inspection report.

> **AI assists with extraction. Rules determine compliance.**

🔗 **Live Demo:** https://sih-packaged-commodity-compliance.vercel.app/

---

## 🎯 Problem

Packaged commodities sold through retail stores, supermarkets, and e-commerce platforms are required to display several mandatory declarations, including manufacturer/packer/importer information, net quantity, MRP, manufacturing/packing/import details, consumer-care information, and other prescribed declarations.

Manually checking these declarations across a large number of products is:

* ⏱️ Time-consuming
* 👁️ Dependent on manual visual inspection
* 📋 Difficult to document consistently
* 📊 Difficult to track across multiple inspections
* ⚠️ Prone to missed or incorrectly recorded violations

PARAKH addresses this by turning the inspection process into a **digital, AI-assisted workflow**.

---

# 💡 Our Solution

PARAKH combines **multimodal AI** with deterministic regulatory validation.

```text
              PRODUCT / PACKAGE IMAGE
                       │
                       ▼
               ┌─────────────────┐
               │  Gemini Vision  │
               │      API        │
               └────────┬────────┘
                        │
                        ▼
              Structured Declarations
                        │
                        ▼
          ┌───────────────────────────┐
          │ Legal Metrology           │
          │ Compliance Engine         │
          └─────────────┬─────────────┘
                        │
              ┌─────────┴─────────┐
              ▼                   ▼
          COMPLIANT            VIOLATIONS
                                  │
                                  ▼
                         Compliance Report
```

The important architectural decision is that **Gemini is not responsible for making the final legal/compliance decision**.

It extracts and structures information from the image.

The compliance engine applies predefined rules to that structured information.

---

# ✨ Key Features

## 🤖 1. AI-Powered Package Analysis

PARAKH uses the **Gemini Vision API** to analyze package images and identify relevant declarations.

Instead of treating the package as plain text, the model can interpret the visual context of declarations.

For example:

```text
"M.R.P. ₹120/-"

        ↓

Field: MRP
Value: ₹120
```

Similarly:

```text
"Net Weight 500 g"

        ↓

Field: Net Quantity
Value: 500 g
```

---

## ⚖️ 2. Legal Metrology Compliance Engine

The extracted information is passed to a rule-based validation layer.

The engine checks relevant declarations such as:

* Manufacturer / Packer / Importer
* Address
* Net Quantity
* MRP
* Manufacturing / Packing / Import information
* Consumer Care details
* Other configured mandatory declarations

The system can identify missing or non-compliant fields instead of simply returning a generic pass/fail result.

---

# 🛡️ 3. AI + Rule Engine Architecture

A major design principle of PARAKH is the separation between **AI extraction** and **regulatory validation**.

### ❌ We don't do this:

```text
Image → Gemini → "Compliant"
```

Because an LLM could potentially interpret a regulation incorrectly or generate unsupported conclusions.

### ✅ PARAKH does this:

```text
Image
  ↓
Gemini Vision
  ↓
Structured Information
  ↓
Deterministic Rules
  ↓
Compliance Result
```

This makes the system more **controlled, explainable, and suitable for human review**.

---

# 👮 4. Inspector Workflow

PARAKH is designed around a digital inspection workflow.

```text
LOGIN
  ↓
Inspector Dashboard
  ↓
Upload Product Image
  ↓
AI-Based Visual Analysis
  ↓
Extract Mandatory Declarations
  ↓
Run Compliance Check
  ↓
Identify Violations
  ↓
Review Result
  ↓
Generate Report
```

This reduces the amount of repetitive manual checking and documentation required during an inspection.

---

# 🖥️ 5. Inspector Dashboard

The inspector-facing interface provides a centralized workspace for performing compliance inspections.

An inspector can:

* Access the inspection workflow
* Submit product/package information
* Analyze packaging
* Review extracted declarations
* View compliance results
* Identify violations
* Generate reports
* Review inspection information

---

# 🛠️ 6. Admin Panel

PARAKH also includes an administrative interface for managing the compliance platform.

The admin functionality provides centralized access to areas such as:

* User management
* Inspector management
* Compliance rules
* Inspection information
* System-level monitoring

This separates the **operational inspection workflow** from **system administration**.

---

# 📄 7. Automated Report Generation

Once an inspection is completed, PARAKH generates a structured compliance report.

The report can contain:

* Product information
* Extracted declarations
* Compliance status
* Detected violations
* Inspection information
* Supporting details

This converts the result of an AI-assisted inspection into a **documented digital record**.

---

# 🔐 8. Authentication

The application provides authenticated access to protected workflows.

The backend exposes authentication APIs and uses token-based authentication to control access to application resources.

This ensures that inspection and administrative functionality isn't treated as an unrestricted public API.

---

# 🧠 Why Gemini Vision Instead of Traditional OCR?

Traditional OCR primarily answers:

> **"What characters are present in this image?"**

PARAKH requires a more contextual question:

> **"What declaration is present here, and what does it represent?"**

For example, a package might contain:

```text
M.R.P. ₹120/-
```

A conventional OCR pipeline may return:

```text
M.R.P. 120
```

PARAKH needs to map that information into a meaningful structured field:

```json
{
  "field": "MRP",
  "value": "₹120"
}
```

Gemini Vision provides multimodal image understanding that can assist with this extraction.

However, **AI output is not treated as the regulatory authority**.

The extracted information is subsequently evaluated by the compliance engine.

---

# 🏗️ System Architecture

```text
                         ┌──────────────┐
                         │    User      │
                         │ Inspector /  │
                         │    Admin     │
                         └──────┬───────┘
                                │
                                ▼
                     ┌────────────────────┐
                     │   React Frontend   │
                     │       + Vite       │
                     └─────────┬──────────┘
                               │
                            REST API
                               │
                               ▼
                     ┌────────────────────┐
                     │   Node.js +        │
                     │   Express Backend  │
                     └───────┬────────────┘
                             │
             ┌───────────────┼────────────────┐
             │               │                │
             ▼               ▼                ▼
      ┌────────────┐  ┌──────────────┐  ┌──────────┐
      │ Gemini     │  │ Compliance   │  │  SQLite  │
      │ Vision API │  │ Engine       │  │ Database │
      └────────────┘  └──────┬───────┘  └──────────┘
                              │
                              ▼
                     Compliance Result
                              │
                              ▼
                     Report Generation
```

---

# 🛠️ Technology Stack

| Layer                  | Technology               |
| ---------------------- | ------------------------ |
| Frontend               | React.js                 |
| Build Tool             | Vite                     |
| Backend                | Node.js                  |
| API Framework          | Express.js               |
| Database               | SQLite                   |
| AI / Vision            | Gemini Vision API        |
| Authentication         | JWT-based authentication |
| Charts / Visualization | Recharts                 |
| HTTP Client            | Axios                    |
| Icons                  | Lucide React             |
| Report Generation      | jsPDF / HTML Canvas      |
| Deployment             | Vercel + Render          |

---

# 📁 Project Architecture

```text
sih-packaged-commodity-compliance/
│
├── client/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── services/
│   │   ├── routes/
│   │   └── ...
│   │
│   └── package.json
│
├── server/
│   ├── routes/
│   ├── controllers/
│   ├── services/
│   ├── database/
│   ├── middleware/
│   └── ...
│
├── README.md
└── ...
```

---

# 🔄 End-to-End Processing Pipeline

### 1. Image Input

The inspector provides a product/package image.

### 2. Visual Analysis

The image is sent to the Gemini Vision API.

### 3. Information Extraction

Relevant package declarations are identified and converted into structured information.

### 4. Compliance Evaluation

The structured information is passed through the Legal Metrology compliance engine.

### 5. Violation Detection

The engine checks the extracted fields against configured regulatory requirements.

### 6. Result Generation

The system produces a compliance result containing the detected status and violations.

### 7. Report Generation

The inspection information can be converted into a structured report for documentation.

---

# 📊 Example Compliance Evaluation

```text
┌──────────────────────────────────┐
│       COMPLIANCE INSPECTION      │
├──────────────────────────────────┤
│                                  │
│ Manufacturer        ✓ Present    │
│ Address             ✓ Present    │
│ Net Quantity        ✓ Present    │
│ MRP                 ✗ Missing    │
│ Manufacturing Date  ✓ Present    │
│ Consumer Care       ✗ Missing    │
│                                  │
├──────────────────────────────────┤
│ Result: NON-COMPLIANT            │
└──────────────────────────────────┘
```

Instead of only showing a final verdict, PARAKH identifies the **specific fields that require attention**.

---

# 🌟 What Makes PARAKH Different?

### 01 — AI-Assisted, Not AI-Decided

AI handles visual understanding and extraction while deterministic rules handle compliance evaluation.

### 02 — Inspector-Centric

The platform is designed around the workflow of a regulatory inspector rather than simply being an OCR demonstration.

### 03 — Explainable Results

The system can show which declarations were detected and which requirements were violated.

### 04 — Digital Documentation

Inspection results can be transformed into structured reports rather than remaining as manual notes.

### 05 — Extensible Compliance Engine

Rules are separated from the visual extraction process, allowing additional compliance checks to be introduced without redesigning the entire AI pipeline.

---

# 🔒 Security & Configuration

API keys and authentication secrets should be provided through environment variables.

Example:

```env
GEMINI_API_KEY=your_gemini_api_key
JWT_SECRET=your_jwt_secret
CLIENT_URL=your_frontend_url
```

**Never commit real API keys or secrets to GitHub.**

---

# 🚀 Running Locally

## Prerequisites

* Node.js
* npm
* Git
* Gemini API key

## Clone the repository

```bash
git clone -b praman-prototype https://github.com/Nidhi-2107/sih-packaged-commodity-compliance.git

cd sih-packaged-commodity-compliance
```

## Backend

```bash
cd server
npm install
```

Configure the required environment variables and start the backend:

```bash
npm run dev
```

## Frontend

Open another terminal:

```bash
cd client
npm install
npm run dev
```

The frontend can then be accessed through the local Vite development server.

---

# 🌐 Deployment

The current prototype uses a separate frontend and backend deployment architecture.

```text
                 Internet
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
       Vercel              Render
          │                   │
          ▼                   ▼
      React UI          Node / Express
                              │
                     ┌────────┴────────┐
                     ▼                 ▼
                  Gemini             SQLite
                   API              Database
```

### Live Application

**PARAKH:**
https://sih-packaged-commodity-compliance.vercel.app/

---

# 🎯 SIH Problem Statement

**Problem Statement ID:** 26034

**Title:** Software System to check compliance of Packaged Commodities under Legal Metrology (Packaged Commodities) Rules, 2011 by scanning products, images and labels.

**Organization:** Ministry of Consumer Affairs, Food & Public Distribution

**Category:** Software

The official problem statement calls for automated scanning and analysis of packaged commodity labels, detection and validation of mandatory declarations, identification of non-compliant declarations, compliance reports, inspection history, and dashboards for enforcement officials.

PARAKH addresses these requirements through an integrated **AI-assisted inspection and rule-based compliance workflow**.

---

# 🔮 Future Scope

Potential extensions include:

* 📱 Mobile application for field inspectors
* 📷 Real-time camera-based scanning
* 🌐 E-commerce product-listing compliance
* 🌍 Improved multilingual package analysis
* 📊 Advanced inspection analytics
* 🗺️ Geographic inspection visualization
* 🔔 Automated violation notifications
* 🔄 Versioned regulatory rule updates
* ☁️ Scalable cloud infrastructure
* 📈 Historical compliance trends
* 🧾 Expanded report and evidence management

---

# ⚠️ Prototype Disclaimer

PARAKH is an **SIH prototype and decision-support system**.

AI-generated extraction should be reviewed by an authorized inspector before being used for any regulatory or enforcement action.

The system is intended to **assist human inspection**, not replace statutory authority or official regulatory judgment.

---

# 👥 Team

Developed for **Smart India Hackathon 2026** under the Legal Metrology packaged-commodity compliance problem statement.

---

## ⭐ PARAKH

> **Scan. Extract. Verify. Comply.**

An AI-assisted approach to making packaged-commodity compliance inspection **faster, structured, explainable, and digitally traceable**.

