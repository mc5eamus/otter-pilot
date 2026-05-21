# otter-pilot

School homework tracing from classroom whiteboard photos.

## Brainstorming plan: homework capture + prioritization

### Goal
Let a student take a whiteboard photo, extract homework items (subject, task title, pages, due hints), compare with the lesson plan, and get clear priorities/urgency.

### Option A (fast MVP, cloud-first on Azure)
- Capture: responsive web app (PWA shell) used from phone browser.
- OCR: Azure AI Vision Read API.
- Structuring + suggestions: Azure OpenAI (JSON schema output).
- Storage: Azure Table Storage or Cosmos DB.
- Prioritization: simple rules first (due date proximity, effort estimate, missing details), optional LLM explanation.
- Offline: cache UI + pending uploads in browser (service worker + IndexedDB), sync when online.

### Option B (privacy/offline-first, then cloud sync)
- Capture + OCR locally in browser/app using on-device OCR (e.g., Tesseract.js or native OCR where available).
- Local-first task store in IndexedDB/SQLite.
- Optional cloud sync to Azure when available.
- Cloud LLM used only for optional enrichment (rewrites, study tips, conflict detection).

### Option C (hybrid quality approach)
- On-device quick OCR for instant result.
- Background cloud re-processing for higher accuracy + normalization.
- Reconciliation flow if cloud result differs from local extraction.

### Suggested phased implementation
1. **Phase 1 (2-3 weeks):** PWA capture, cloud OCR, manual edit screen, homework list, simple priority scoring.
2. **Phase 2:** lesson-plan import, duplicate detection, reminders.
3. **Phase 3:** offline queue/sync hardening, multi-photo day history, parent/child shared view.

### Open decisions
1. Offline level target:
   - Read-only offline, or full create/edit/sync offline?
2. OCR strategy:
   - Cloud-only vs hybrid vs fully local fallback.
3. Data model:
   - Strict structured fields only, or keep raw extracted text + structured projection.
4. Privacy/compliance:
   - Photo retention duration and redaction policy.
5. Priority UX:
   - Rule-based ranking only, or LLM-generated rationale and study plan.
6. Deployment shape on Azure:
   - Static Web Apps + Functions vs App Service/API backend.

### Minimal architecture recommendation
- **Frontend:** PWA (React or plain TypeScript web app).
- **Backend:** Azure Functions (HTTP endpoints for OCR + normalization + sync).
- **AI services:** Azure AI Vision + Azure OpenAI.
- **Storage:** Cosmos DB (tasks + extraction metadata), Blob Storage (optional photo archive).
- **Ops:** App Insights telemetry, feature flags for OCR/LLM strategy.
