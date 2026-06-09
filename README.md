# Project
Assessment 
Implementation roadmap — Detailed milestones, tasks, dependencies, and estimated effort

Assumptions
- Prototype is standalone (no integration with COLA).
- Hosted on Azure (per org context) unless you specify otherwise.
- No sensitive data storage for prototype; uploads ephemeral.
- Use open-source OCR (Tesseract) + optional cloud ML for improved extraction if firewall allows.
- Team: 1 product owner (PO), 1 tech lead, 2 backend engineers, 1 frontend engineer, 1 ML engineer, 1 QA, 1 UX designer. Adjust estimates if team size differs.

Summary timeline
- Total: 8–10 weeks (MVP for agent workflow + basic batch and UX). Phases: Discovery (1 wk), Infra + Core OCR (2 wk), Extractors & Rules (2 wk), UI + Batch + Performance (2 wk), QA & Launch (1–2 wk).

Milestone 0 — Discovery & setup (1 week)
- Goals: finalize scope, success criteria, test data, environment constraints (firewall/API), repo and CI.
- Tasks:
  - PO: confirm required fields & validation rules (Sarah/Jenny input). (1 day)
  - Tech lead: confirm deployment constraints & Azure subscription, networking/firewall limitations. (1 day)
  - ML engineer + UX: collect/generate 30 representative label images (simple + noisy). (2 days)
  - Setup repo, CI pipeline, basic README. (1 day)
- Deliverables: project plan, sample dataset, repo skeleton.
- Effort: 5 person-days.

Milestone 1 — Infra & Ingestion API + Image storage (1 week)
- Goals: enable fast uploads, single and batch, ephemeral storage.
- Tasks:
  - Backend: implement upload endpoints (single, batch), validation, and storage (blob with TTL). (3 days)
  - Backend: implement lightweight job queue for processing (Redis/RabbitMQ or Azure Queue). (1 day)
  - DevOps: deploy dev environment on Azure (App Service or containers + Blob Storage). (1 day)
- Deliverables: upload API, queue wiring, deployed dev env.
- Effort: 7 person-days.

Milestone 2 — OCR pipeline (2 weeks)
- Goals: reliable, fast text extraction with fallback strategy for noisy images.
- Tasks:
  - ML: integrate Tesseract OCR as primary local engine, tuned for label fonts and layout. (3 days)
  - ML: implement pre-processing (deskew, denoise, contrast adjust) pipeline using OpenCV. (3 days)
  - ML: optional Cloud OCR adapter (Azure Cognitive Services) with toggle for environments that allow outbound calls. (2 days)
  - Backend: orchestrate OCR jobs, cache results, return within target latency (<=5s per simple label). (2 days)
- Deliverables: OCR service, pre-processing module, adapter switch, performance benchmark.
- Effort: 14 person-days.

Milestone 3 — Field extraction & validation rules (2 weeks)
- Goals: map OCR output to required fields and apply TTB validation rules (exact government warning, ABV format, net contents, etc.).
- Tasks:
  - ML/backend: implement field extraction heuristics:
    - Regex-based parsers for ABV, net contents, proof. (2 days)
    - Named-entity / layout heuristics for brand, class/type, bottler address. (3 days)
  - Rules engine: encode validation rules with severity levels (pass, warn, fail) and fuzzy matching thresholds for brand name (to support Dave’s examples). (3 days)
  - UI contract: design JSON response format for frontend (fields, confidences, snippets, bounding boxes). (2 days)
- Deliverables: extraction service, validation engine, API spec.
- Effort: 14 person-days.

Milestone 4 — Agent UI & UX (2 weeks)
- Goals: simple, clear interface for agents (single-label review + batch), accessible for low-tech users.
- Tasks:
  - UX designer: create low-fidelity wireframes and a simple high-contrast UI with large buttons and explicit checklist flow. (3 days)
  - Frontend: implement label review screen: image, auto-filled fields with confidence, quick accept/edit field inline, highlight mismatches. (5 days)
  - Frontend: implement batch upload view with queue and bulk-accept workflows. (3 days)
  - Frontend/backend: implement quick keyboard shortcuts and 5-second target for simple accept flow. (2 days)
- Deliverables: working frontend prototype, usability-first layout.
- Effort: 15 person-days.

Milestone 5 — Performance, UX polish, and accessibility (1 week)
- Goals: meet the 5-second responsiveness requirement, make UI accessible, handle edge cases.
- Tasks:
  - ML/backend: optimize pipeline (caching, parallelism), measure latency, set timeouts and graceful fallbacks. (3 days)
  - Frontend: accessibility audit (large fonts, high contrast, simple navigation), minor UI polish. (2 days)
- Deliverables: performance report, accessibility checklist met.
- Effort: 7 person-days.

Milestone 6 — QA, user testing, & iterations (1–2 weeks)
- Goals: bug fixes, refine fuzzy matching thresholds, incorporate feedback from 3–5 agents.
- Tasks:
  - QA: automated tests (unit, integration), regression tests on dataset. (3 days)
  - Conduct 2 rounds of user testing with agents (observe tasks, measure time saved, collect feedback). (3–5 days)
  - Implement prioritized fixes and tweak rules. (2–5 days)
- Deliverables: tested MVP, user feedback document, final adjustments.
- Effort: 10–20 person-days.

Milestone 7 — Demo, docs, and handoff (3 days)
- Goals: deploy a stable demo instance, prepare README and runbook, and deliver presentation.
- Tasks:
  - DevOps: deploy demo instance, configure access controls for reviewers. (1 day)
  - PO/Tech lead: prepare demo script, short video, and runbook for next steps. (1 day)
  - Repo: finalize README, architecture notes, and assumptions. (1 day)
- Deliverables: live demo URL, docs, handoff package.
- Effort: 3 person-days.

Total estimated effort (rounded)
- Core MVP: ~57–80 person-days depending on iteration cycles and cloud OCR inclusion.
- With a 6-person implementation team working in parallel, calendar time ~8–10 weeks.

Risks & mitigation
- Firewall blocks cloud OCR: provide local OCR fallback (Tesseract) and design adapter to enable cloud OCR where permitted.
- Latency >5s for real-world noisy images: optimize pre-processing, add lightweight confidence-based fast-path (if high confidence accept), and queue heavy processing for manual review.
- False positives/negatives on brand matching: expose fuzzy-match threshold to agents, allow easy override and feedback loop to retrain heuristics.
- Accessibility/tech comfort: conduct quick usability sessions with older agents (e.g., Dave) early and iterate.

