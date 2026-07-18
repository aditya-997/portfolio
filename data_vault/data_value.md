## 🔒 DataVault
**A Secure, Content-Addressable Personal File Storage Vault & Media Metadata Indexer.**

*   **Codebase Status:** Private
*   **Architecture:** Decoupled Spring Boot Rest API (Java 17) + React 18 / Tailwind v4 Client (Vite)

### 🚀 Key Technical Features

*   **Client-Side Hashing & Instant Deduplication (CAS):** 
    Before uploading raw bytes, the frontend computes a SHA-256 hash using the browser's native **Web Crypto API** (with a fallback to `js-sha256`). It performs a pre-flight index check. If the hash already exists on the server, the file is instantly linked to the user's folder metadata without uploading the file again.
*   **Asynchronous Multi-Threaded File Processing:**
    Uploads are stored temporarily (`status: TEMP`) and processed in background worker threads using Spring's `@Async` executor to prevent blocking HTTP request cycles.
*   **EXIF Metadata Extraction:**
    Images are parsed using `metadata-extractor` to capture photographic metadata (EXIF). The app dynamically updates the file's primary creation date based on when the photo was originally taken, rather than the upload date.
*   **PDF Thumbnail Rendering:**
    PDF uploads are processed using **Apache PDFBox**, which loads the document, extracts the cover page (Page 0) at 100 DPI, and scales it maintaining the aspect ratio to generate a `256x256` preview thumbnail.
*   **Structured Vault Storage:**
    Files are moved atomically (`StandardCopyOption.ATOMIC_MOVE`) into a structured year/month/day folder hierarchy (`/uploads/vault/YYYY/MM/DD/type/[hash].[extension]`) matching their resolved creation dates.
*   **Self-Healing File Tracker:**
    If a temporary upload job fails or is interrupted, a recovery routine walks the vault disk structure using Java NIO streams, verifies files by SHA-256 hash, and restores database state records dynamically.

### 🛠️ Tech Stack
*   **Frontend:** React 18, Vite 5, Tailwind CSS v4, Lucide Icons, Web Crypto API
*   **Backend:** Spring Boot 3.4, Spring Data JPA, Java 17
*   **Database:** SQLite (Relational indexing) with Hibernate Dialect mapping
*   **Processing Engines:** Apache PDFBox (PDF Rendering), Drew Noakes' Metadata Extractor (EXIF parsing)

---

### 📐 Database Schema & Deduplication Model

The database achieves **Single-Instance Storage (SIS)**. Multiple users can upload the exact same file in different folders, but only a single copy is kept on disk.

```mermaid
classDiagram
    class UserFileEntity {
        +Long id
        +String userName
        +String originalName
        +LocalDate uploadDate
        +LocalDateTime uploadedAt
        +FolderEntity folder
        +FileContentEntity fileContent
    }
    class FileContentEntity {
        +Long id
        +String storedPath
        +String fileHash [Unique SHA-256]
        +String fileType [image/pdf/excel/word-txt]
        +long sizeBytes
        +String mimeType
        +LocalDate fileCreatedDate [EXIF/JS Fallback]
        +String fileMetadata [EXIF tag JSON]
        +String status [TEMP/UPLOADING/COMPLETED]
    }
    class FolderEntity {
        +Long id
        +String name
        +String userName
        +FolderEntity parent
    }
    UserFileEntity --> FileContentEntity : Many-to-One (Deduplicated content link)
    UserFileEntity --> FolderEntity : Many-to-One
    FolderEntity --> FolderEntity : Parent-Child Relation
