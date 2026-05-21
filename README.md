# otter-pilot

School homework tracing from classroom whiteboard photos.

## Brainstorming plan: homework capture + prioritization

### Goal
Let a student take a whiteboard photo, extract homework items (subject, task title, pages, due hints), compare with the lesson plan, and get clear priorities/urgency.

### Selected direction: full cloud option on Azure
- Capture: responsive web app (PWA shell) used from phone browser.
- Offline capture: allow taking pictures while offline and store pending uploads in browser storage (service worker + IndexedDB).
- Sync: automatically upload queued photos and metadata once connectivity is back.
- OCR: Azure AI Vision Read API (cloud processing).
- Structuring + suggestions: Azure OpenAI (JSON schema output).
- Storage: Cosmos DB for homework items and processing state, Blob Storage for photo files.
- Prioritization: start with deterministic urgency score (due date proximity, effort estimate, missing details), optionally add LLM explanation text.

### Suggested phased implementation
1. **Phase 1 (2-3 weeks):** PWA capture, cloud OCR, manual edit screen, homework list, simple priority scoring.
2. **Phase 2:** lesson-plan import, duplicate detection, reminders.
3. **Phase 3:** offline queue/sync hardening, multi-photo day history, parent/child shared view.

### Open decisions (within the full cloud path)
1. Offline queue limits:
   - Maximum number/size of photos queued before sync is required.
2. Data model:
   - Strict structured fields only, or keep raw extracted text + structured projection.
3. Privacy/compliance:
   - Photo retention duration, parental controls, and redaction policy.
4. Priority UX:
   - Rule-based ranking only, or LLM-generated rationale and study plan.
5. Deployment shape on Azure:
   - Static Web Apps + Functions vs App Service/API backend.

### Minimal architecture recommendation (full cloud)
- **Frontend:** PWA (React or plain TypeScript web app).
- **Backend:** Azure Functions (HTTP endpoints for upload, OCR orchestration, normalization, sync status).
- **AI services:** Azure AI Vision + Azure OpenAI.
- **Storage:** Cosmos DB (tasks + extraction metadata + sync states), Blob Storage (photo archive).
- **Ops:** App Insights telemetry, retry/dead-letter strategy for failed OCR jobs, feature flags for ranking strategy.
