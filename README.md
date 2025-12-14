mermaid_pipeline = r"""
flowchart LR
    A[Start: Upload Provider PDF/Scan] --> B[Orchestrator]

    B --> C{Digital Text Available?}

    C -- YES --> D1[pdfplumber Extraction]
    C -- NO --> D2[Convert to Images (pdf2image)]
    D2 --> D3[Image Preprocessing]
    D3 --> D4[Tesseract OCR]
    
    D1 --> E[Extracted Text Sent to Orchestrator]
    D4 --> E

    E --> F[Entity Extraction<br>(Names, Addresses, NPI)]
    F --> G[Address Normalization<br>(usaddress → scourgify)]
    G --> H[Fuzzy Matching<br>RapidFuzz]
    H --> I{NPI Found?}

    I -- YES --> J[NPPES API Lookup by NPI]
    I -- NO  --> K[Fallback Search: Name + Location]

    J --> L[Flatten Registry Response]
    K --> L

    L --> M[Compare Extracted Data vs Registry Data]
    M --> N[Compute Match Confidence Score]

    N --> O[Great Tables Credentialing Report]
    N --> P[Panel Dashboard Update]
    N --> Q[Rich Log Output]

    O --> R[End: Simulation Output]
    P --> R
    Q --> R
"""
print(mermaid_pipeline)
