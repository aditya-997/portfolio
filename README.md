# Software Engineering Portfolio & Project Case Studies

Welcome to my portfolio! This repository acts as a public showcase for the systems and applications I have designed and built. 

To respect proprietary work and maintain clean code security, the underlying source codes for these projects are hosted in private repositories. However, you can find detailed breakdowns of their system architectures, design decisions, database schemas, and technical challenges by navigating the folders below.

---

## 📁 Featured Projects

### 1. 🔒 [DataVault](./data_valult)
**A Secure, Content-Addressable Personal File Storage Vault & Media Metadata Indexer.**
*   **Status:** Case Study Available | Codebase Private
*   **Core Tech:** Spring Boot 3.4 (Java 17), SQLite, React 18, Tailwind CSS v4, Apache PDFBox, Metadata Extractor.
*   **Key Highlights:** 
    *   **Instant Hashing Deduplication (CAS):** Computes SHA-256 on the client side via the Web Crypto API to avoid redundant uploads.
    *   **Background Worker Threads:** Processes files asynchronously using Spring `@Async` logic.
    *   **Media Parsing:** Automatic EXIF metadata parsing for photos and high-DPI first-page thumbnail rendering for PDFs.
*   **Read the full breakdown:** 👉 **[Explore DataVault Case Study](./data_valult)**

### 2. 🎙️ [Speech2Sheets (S2S)](./Speech2Sheets)
**An Interactive, Voice-Controlled Spreadsheet & Calculation Engine.**
*   **Status:** Case Study Available | Codebase Private
*   **Core Tech:** React, Tailwind CSS, Web Speech API (Speech-to-Text), Node.js/Express, Relational Database.
*   **Key Highlights:**
    *   **Voice-Driven Spreadsheet Operations:** Real-time speech recognition allowing hands-free cell navigation, sheet editing, and formula calculation.
    *   **Natural Language Parser:** Translates spoken English commands (e.g., *"Sum columns A and B in row 5"*) into structured grid actions and math expressions.
    *   **Reactive Calculations:** Custom client-side grid rendering with cell dependency trees for real-time recalculations.
*   **Read the full breakdown:** 👉 **[Explore Speech2Sheets Case Study](./Speech2Sheets)**

---

## 🛠️ Combined Skill Matrix

*   **Frontend Engineering:** React (v18+), Vite, Next.js, Modern CSS (Tailwind v4, Glassmorphism, Responsive Grid Systems), Web Crypto API, Web Speech API.
*   **Backend & APIs:** Java 17, Spring Boot, REST APIs, Node.js, Express.
*   **Data & Storage:** SQLite, PostgreSQL, JPA/Hibernate, Content-Addressable Storage (CAS) design, Local Vault Structures.
*   **Utilities & System Operations:** PDF rendering engines, EXIF metadata extraction, multi-threaded background processing.
