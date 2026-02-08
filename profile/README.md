# Quishing Detection System – GitHub Organization

This GitHub organization hosts the source code for a multi-stage **QR-code phishing (Quishing) detection system**. The goal is to analyze QR codes and their target URLs in a secure, sandboxed environment and provide a transparent **risk assessment** to end users.[file:1]

---

## Repositories

### 1. `qre-frontend`

Modern web frontend for interacting with the Quishing detection API.

- Built with **Vite**, **React**, **TypeScript**, **Tailwind CSS**, and **Material UI**.  
- Allows users to:
  - Upload QR-code images or PDFs.
  - Submit plain URLs for analysis.
  - View risk scores, categories (e.g. “safe”, “suspicious”, “dangerous”) and detailed reasons.[file:1]

Typical responsibilities:

- User-facing scan form and result views.  
- Visualisation of redirect chains, threat intelligence hits, and detected phishing patterns.  
- Communication with the backend via REST API.

Frontend repo: `qre-frontend` (see that repository’s README for setup and scripts).

---

### 2. `qre-backend`

Backend services implementing the core Quishing detection pipeline.

Main components:[file:1]

- **QR code decoding** (image/PDF → URL).  
- **Static URL & domain analysis**  
  - Redirect tracking  
  - TLS certificate inspection  
  - TLD and domain age checks  
  - Short-link and obfuscation detection  
- **Threat intelligence integration**  
  - External services such as Google Safe Browsing, VirusTotal, and phishing feeds. 
- **Dynamic analysis (sandbox)**  
  - Headless browser inspection of the final landing page  
  - Detection of login forms, credential harvesting UIs, and other social-engineering indicators.
- **Risk scoring engine**  
  - Aggregates all features into a numerical score and discrete risk level.  

The backend exposes a REST API consumed by `qre-frontend` and can also be integrated by external systems (e.g. mail gateways).

Backend repo: `qre-backend` (see that repository’s README for environment, database, and Prisma setup).

---

## High-Level Architecture

At a high level, the system consists of:

- A **web UI** (`qre-frontend`) for manual scans and result exploration.  
- A **REST API** (`qre-backend`) orchestrating:
  - QR decoding,
  - static URL/domain checks,
  - threat-intel lookups,
  - optional sandboxed dynamic analysis,
  - final risk scoring and persistence in a database.

Both repositories are designed to be developed and deployed together but remain loosely coupled so they can evolve independently.

---

## Getting Started

1. Clone both repositories into a common workspace:
   ```bash
   git clone https://github.com/QR-Code-Evaluation//qre-backend.git
   git clone https://github.com/QR-Code-Evaluation//qre-frontend.git
   ```

2. Follow the setup instructions in each repo’s README:

- Backend: install dependencies, configure environment variables (e.g. DATABASE_URL, API keys), run database migrations, start NestJS server.
- Frontend: install dependencies, configure VITE_API_URL, start Vite dev server.

Once both services are running, open the frontend in your browser and start scanning QR codes or URLs through the unified interface.
