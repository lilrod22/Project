# Project
Assessment 
TTB Label Review Prototype

Brief proof-of-concept web app that automates routine label verification checks (brand name, ABV, net contents, government warning, producer info) from uploaded label images and application metadata. Built as a standalone prototype to inform future integration with the COLA system.

Key features
￼   OCR-based extraction of label text
￼   Rule checks for required fields (brand, class/type, alcohol content, net contents, producer, government warning)
￼   Exact-match and fuzzy-match modes (configurable tolerance) to handle capitalization/format variants
￼   Batch upload processing
￼   Simple, senior-friendly UI (large buttons, minimal options)
￼   Lightweight REST API for processing images

Tech stack
￼   Backend: Python (FastAPI), Tesseract OCR (pytesseract) for on-prem OCR, optional ML model hooks (placeholder)
￼   Frontend: React with simple accessible components
￼   Storage: No persistent PII storage in prototype; temporary filesystem processing
￼   Dev / hosting: Docker for local dev; optional container deployment to Azure (prototype only)

Repository structure (high level)
￼   backend/ — FastAPI app, processing logic, OCR wrappers, validation rules
￼   frontend/ — React app (build served by backend in prod)
￼   tests/ — unit and integration tests
￼   samples/ — sample label images and JSON application payloads
￼   docs/ — additional design notes and assumptions
￼   docker-compose.yml, README.md

Quickstart (local, development)
1.	Clone the repo:
   git clone <repo-url>
2.	Start with Docker Compose:
   docker-compose up --build
3.	Frontend UI available at:
   ￼
4.	API docs (OpenAPI) available at:
   ￼

Alternative (no Docker):
￼   Backend:
  cd backend
  python -m venv .venv
  source .venv/bin/activate
  pip install -r requirements.txt
  uvicorn app.main:app --reload --port 8000
￼   Frontend:
  cd frontend
  npm install
  npm start

Environment variables
￼   OCR_ENGINE=tesseract
￼   PROCESS_BATCH=true|false
￼   FUZZY_MATCH_THRESHOLD=0.85
￼   PORT_BACKEND=8000
￼   PORT_FRONTEND=3000

(See backend/.env.example for full list.)

Usage
￼   Single image: Upload a label image and the application JSON; the app returns extracted fields and pass/fail checks with explanations.
￼   Batch: Upload a zip of images and a CSV/JSON manifest to process many applications at once.

Sample data
￼   samples/labels/ contains example distilled spirits, wine, and beer labels (photographed and synthetic).
￼   samples/manifests/ contains example application payloads to test matching behavior.

Design & assumptions
￼   Prototype does not integrate directly with COLA or any production TTB systems.
￼   No long-term storage of images or PII in prototype mode.
￼   OCR chosen for offline reliability (avoids outbound network restrictions described by IT).
￼   Performance target: per-image processing under 5s on modest hardware; use lightweight OCR pipeline and configurable preprocessing (deskew, contrast).
￼   Human-in-the-loop: results shown to agent for quick accept/reject and manual override for nuanced cases (e.g., punctuation/casing differences).

Security & compliance notes
￼   For prototype/demo only. Do not use for production PII handling without FedRAMP / agency security review and hardened deployment.
￼   Network: prototype avoids external ML API calls by default to accommodate restricted outbound networks.

Tests
￼   Run backend tests:
  cd backend
  pytest -q
￼   Frontend unit tests:
  cd frontend
  npm test

Known limitations & future work
￼   OCR errors on angled/glare-heavy photos; future work: add image enhancement and a small fine-tuned OCR model.
￼   Better NLP for matching brand aliases and trade names.
￼   Integration plan for COLA requires procurement, auth, and FedRAMPed hosting — out of scope for this prototype.

Demo / Deployment
￼   Deployed App URL: (insert deployed URL here)
￼   If you want access to a live demo, run the quickstart locally or contact the maintainer.

How to evaluate
￼   Test with samples/ and your own label images.
￼   Try edge cases: casing differences, punctuation, small-font warnings, angled photos, and batch uploads.

README / documentation updates
￼   See docs/design.md for detailed validation rules (government warning exact text, ABV formats accepted, net contents parsing).

Contact / Maintainers
￼   Project lead: [name] — see repo CONTRIBUTORS file.
