# AI MYSTERY ROOM
## Product Specification & Technical Source of Truth

**Document:** `SPEC.md`  
**Project:** AI Mystery Room  
**Version:** 1.0.0  
**Status:** Initial development specification  
**Architecture:** MERN Stack  
**AI Provider:** Google Gemini  
**Primary Purpose:** AI-generated interactive mystery investigation game

---

# 1. PROJECT OVERVIEW

## 1.1 Product Vision

AI Mystery Room is an interactive, AI-powered mystery investigation platform where users become detectives and solve fictional cases generated from their own story ideas.

The user submits a short scenario, story, or mystery theme. The application uses Google Gemini to generate a complete investigation containing suspects, a culprit, a timeline, clues, evidence, witness statements, and a final solution.

Users investigate the case by reviewing evidence such as:

- WhatsApp-style conversations.
- AI-generated photographs and crime-scene images.
- Fictional CCTV recordings or reconstructed video evidence.
- Witness statements and suspect interviews.
- Digital documents and reports.
- Timelines and alibis.
- Contradictions between different pieces of evidence.

After investigating, the user identifies the culprit and submits a final accusation. The application evaluates the answer and reveals the solution, including an explanation of how the evidence establishes the culprit.

The experience should feel like an immersive detective investigation rather than a conventional question-and-answer chatbot.

## 1.2 Core Objective

Build a complete, responsive, full-stack website where users can:

1. Create a mystery from their own scenario.
2. Receive an AI-generated investigation.
3. Investigate suspects and evidence.
4. Form a theory using available clues.
5. Accuse a suspect.
6. Discover whether their answer is correct.
7. Read the complete case solution.
8. Review their completed investigations.

## 1.3 Product Principles

- **Fair mysteries:** The culprit must be determined before investigation begins.
- **Consistent evidence:** All clues, statements, timelines, and suspect details must follow the same underlying case facts.
- **No accidental spoilers:** The culprit and solution must never be exposed through ordinary frontend responses.
- **Interactive investigation:** Evidence should be presented through a polished detective interface.
- **AI reliability:** Validate all AI-generated content before making a case playable.
- **Secure by design:** API keys and privileged AI operations must remain on the backend.
- **Resilient experience:** Generation failures must not produce incomplete or contradictory cases.
- **Responsive design:** Support desktop, tablet, and mobile devices.

---

# 2. TARGET USERS

## Primary users

People who enjoy:
- Detective stories and crime mysteries.
- Puzzle-solving and logical reasoning.
- Interactive storytelling.
- Escape-room-style games.
- AI-generated entertainment.

## User types

### Guest user
Can create and investigate mysteries without registering, subject to configurable usage limits.

### Registered user
Can create mysteries, save investigations, view completed cases, and track personal statistics.

### Administrator
Can inspect system health, review failed generations, manage abuse limits, and monitor generation costs through protected administrative functionality.

**MVP decision:** Guest play and registered-user persistence should be supported architecturally. Registration can be implemented as a later phase if it delays the playable core experience. Authentication must not be faked or represented as secure when it is not implemented.

---

# 3. PRODUCT SCOPE

## 3.1 MVP features

The first production-ready version must include:

1. Landing page.
2. Mystery creation form.
3. Gemini-powered case generation.
4. Structured case validation.
5. Suspect directory.
6. Evidence investigation room.
7. Evidence detail viewer.
8. Interactive WhatsApp-style evidence.
9. Image evidence support.
10. CCTV/video evidence representation.
11. Timeline and witness statements.
12. Accusation workflow.
13. Answer validation.
14. Spoiler-safe solution reveal.
15. Investigation progress tracking.
16. Restart and case navigation.
17. Responsive interface.
18. Loading, empty, error, and retry states.
19. Backend persistence using MongoDB.
20. Secure server-side Gemini integration.

## 3.2 Features that may follow the MVP

- User accounts and social login.
- Multiplayer investigations.
- Competitive leaderboards.
- Community-created mystery sharing.
- Daily challenges.
- Difficulty rankings.
- Real video generation using a dedicated media-generation service.
- Voice-based suspect interrogations.
- AI-powered hints.
- Achievement systems.
- Public case galleries.

Do not implement these optional features before the core single-player investigation works reliably.

---

# 4. USER JOURNEY

The primary user journey is:

1. User opens the landing page.
2. User selects **Create a Mystery**.
3. User enters a scenario, selects difficulty, and chooses a setting.
4. The backend validates the request.
5. Gemini generates the canonical case and its evidence.
6. The backend validates consistency and saves the case.
7. The user enters the investigation room.
8. The user reviews suspects, timeline, and evidence.
9. The user records their theory.
10. The user selects a suspect and submits an accusation.
11. The backend evaluates the accusation.
12. The user receives a correct or incorrect result.
13. The complete solution and reasoning become available.
14. The case is marked completed and stored in the user's history.

A user must not be able to see the solution before submitting an accusation or explicitly using an allowed post-investigation reveal action.

---

# 5. INFORMATION ARCHITECTURE

## Required routes

| Route | Purpose |
|---|---|
| `/` | Landing page |
| `/create` | Mystery creation form |
| `/case/:caseId` | Investigation room |
| `/case/:caseId/evidence/:evidenceId` | Evidence detail view |
| `/case/:caseId/accuse` | Final accusation |
| `/case/:caseId/result` | Accusation result and solution |
| `/history` | User's saved investigations |
| `/settings` | User preferences, if implemented |
| `*` | Not-found page |

The frontend may implement evidence details as a modal or side panel instead of a separate visible route, but evidence IDs and direct-link handling must remain consistent.

All protected routes must enforce authorization on the backend, not merely hide UI elements.

---

# 6. DESIGN SYSTEM AND USER EXPERIENCE

## 6.1 Visual direction

Use a premium, cinematic detective-investigation aesthetic.

The interface should feel like a modern digital detective workstation, combining realistic case files, evidence boards, forensic reports, and interactive digital artifacts.

Avoid generic SaaS dashboard styling, excessive gradients, childish illustrations, and cluttered layouts.

## 6.2 Color palette

Use the following initial design tokens:

| Token | Value | Usage |
|---|---|---|
| `background` | `#0B0D10` | Main background |
| `surface` | `#13171D` | Panels and cards |
| `surfaceRaised` | `#1B212A` | Modals and elevated panels |
| `border` | `#303844` | Dividers and card outlines |
| `textPrimary` | `#F3F5F7` | Main text |
| `textSecondary` | `#A6AFBC` | Supporting text |
| `accent` | `#C6A15B` | Investigation highlights |
| `success` | `#53B987` | Correct accusation |
| `danger` | `#E36D70` | Incorrect accusation |
| `info` | `#73A7E8` | Informational indicators |

Use the accent color selectively. Evidence status must also be communicated through text or icons, not color alone.

## 6.3 Typography

Use a readable modern sans-serif font, such as Inter, for the interface.

Optional serif typography may be used for case titles, report headings, and dramatic narrative elements.

Maintain:
- Strong visual hierarchy.
- Readable evidence text.
- Clear timestamps.
- Consistent spacing.
- Accessible contrast.
- Visible keyboard focus states.

## 6.4 Landing page

Required sections:

1. Navigation bar with logo, product name, and primary CTA.
2. Hero section with a compelling mystery-solving headline.
3. Short explanation of AI-generated investigations.
4. Primary action: **Create Your Mystery**.
5. Secondary action: **Explore How It Works**.
6. Feature cards showing evidence types.
7. Three-step explanation of the investigation process.
8. Example mystery preview.
9. Final call to action.
10. Footer with privacy, terms, and fictional-content disclaimer.

Do not display fake customer testimonials, fabricated statistics, or nonfunctional social proof.

## 6.5 Mystery creation page

Include these fields:

- **Scenario:** Required multiline text.
- **Case title:** Optional; generated automatically when blank.
- **Setting:** Modern city, remote location, historical setting, or custom.
- **Mystery category:** Theft, disappearance, fictional homicide, sabotage, cyber mystery, or other non-graphic mystery.
- **Difficulty:** Easy, Medium, Hard.
- **Evidence preference:** Optional selection of available evidence types.
- **Case length:** Short or Standard.

Recommended initial limits:

- Scenario minimum: 30 characters.
- Scenario maximum: 2,000 characters.
- Case title maximum: 100 characters.
- Evidence preference: zero or more supported types.
- Generation request timeout: configurable, with a default budget of approximately 90 seconds.

Validate these limits on both the frontend and backend.

Provide a concise preview of the selected configuration before generation.

The form must explain that AI-generated content may take time to prepare.

## 6.6 Investigation room

This is the core product screen.

Recommended desktop layout:

- **Left sidebar:** Case information, suspect list, evidence categories, progress.
- **Center panel:** Selected evidence, image viewer, chat transcript, or report.
- **Right panel:** Investigation notes, discovered clues, and timeline.

On mobile, use a compact top bar and accessible tabs or drawers.

Required components:

### Case header
- Case title.
- Case ID or short reference.
- Difficulty.
- Investigation progress.
- Evidence count.
- Exit or return-to-history action.

### Suspect directory
Each suspect card shows:
- Name.
- Fictional portrait, when available.
- Occupation or relationship to the case.
- Short public description.
- Known alibi or relevant public information.
- Evidence items connected to the suspect.

Suspect cards must not expose hidden guilt status, secret motivations, or private solution data.

### Evidence board
Display evidence cards with:
- Evidence type.
- Title.
- Short description.
- Source.
- Timestamp, where relevant.
- Related suspect or location.
- Open/review action.
- New or reviewed status.

### Investigation notes
Allow users to record their own theories and observations.

### Accusation CTA
Show a clear **Make Final Accusation** button.

The user should be warned that submitting an accusation ends the active investigation attempt unless the product later supports multiple attempts.

---

# 7. EVIDENCE TYPES

All evidence must be grounded in the same canonical case.

## 7.1 Message evidence

Represent fictional WhatsApp-style conversations using structured message objects.

Fields:
- Sender.
- Recipient or group.
- Timestamp.
- Message text.
- Message sequence.
- Optional reply reference.
- Optional attachment reference.
- Optional delivery/read indicator.

The interface may resemble a messaging application, but it must not imply that these are actual WhatsApp records.

All conversation content must be fictional.

## 7.2 Image evidence

Supported initial image categories:
- Fictional suspect portraits.
- Scene photographs.
- Object photographs.
- Reconstructed location images.
- Visual clues.

Each image evidence item must include:
- Asset ID.
- Image URL or storage reference.
- Alt text.
- Caption.
- Capture or reconstruction timestamp, if relevant.
- Evidence source.
- Related case entities.

Use Gemini image-generation capabilities only when supported by the selected model and API. If image generation is unavailable, use a separate compatible image-generation service or a clearly labeled illustrative placeholder.

Do not assume that a text-only Gemini model can generate image files.

## 7.3 CCTV evidence

The MVP must support CCTV evidence cards even if actual AI-generated video is not yet available.

A CCTV item may contain:
- Video asset URL, if a real generated video exists.
- A sequence of timestamped observations.
- A storyboard or sequence of still frames.
- A transcript of observable events.
- A short summary of relevant activity.

Clearly distinguish between:
- Actual fictional video asset.
- Reconstructed storyboard.
- Textual CCTV report.

Never display a fake video player that suggests playable footage exists when it does not.

## 7.4 Witness statements

Each statement includes:
- Witness ID.
- Statement text.
- Interview timestamp.
- Relevant location.
- Related events.
- Optional contradictions.
- Whether the statement is public or gated.

Contradictions should be intentional, logically explainable, and resolvable through other evidence.

## 7.5 Documents and forensic reports

Examples:
- Incident reports.
- Access logs.
- Receipts.
- Fictional emails.
- Location records.
- Lab findings.
- Inventory records.

Each document must have a title, source, timestamp, body, and optional related entities.

## 7.6 Evidence relationships

Evidence may refer to:
- One or more suspects.
- Locations.
- Events.
- Other evidence items.
- The case timeline.

Relationships help users build theories, but must not automatically reveal whether a suspect is guilty.

The application must not mark evidence as proof of guilt merely because it mentions a suspect.

---

# 8. MYSTERY GENERATION ENGINE

## 8.1 Core principle

The AI must generate a single canonical mystery solution before the user starts investigating.

The culprit, motive, method, timeline, and actual events must be fixed before the evidence is made available.

Evidence generation must be based on those canonical facts rather than independently inventing a culprit or changing the underlying timeline.

## 8.2 Recommended generation pipeline

Use a staged generation process:

1. Validate the user's scenario and configuration.
2. Generate the canonical case outline.
3. Validate the suspect roster and culprit.
4. Generate the event timeline and factual relationships.
5. Generate evidence based on the canonical outline.
6. Run consistency checks across the case.
7. Repair or regenerate invalid evidence when possible.
8. Save the completed case transactionally or mark it ready only after validation.
9. Return a spoiler-safe case response to the frontend.

Use separate Gemini calls when necessary to stay within token limits and improve consistency.

Do not generate the entire case through multiple unconstrained prompts that can independently change the culprit.

## 8.3 Difficulty levels

### Easy
- 3 suspects.
- 6–8 evidence items.
- Direct clues.
- One or two misleading clues.
- Clear final explanation.

### Medium
- 4–5 suspects.
- 9–12 evidence items.
- Multiple interacting clues.
- Conflicting statements.
- At least two evidence types required to resolve the mystery.

### Hard
- 5–6 suspects.
- 13–16 evidence items.
- Interconnected timelines.
- More subtle contradictions.
- Multiple plausible initial theories.
- A solution that requires combining several independent clues.

These are initial product defaults, not guaranteed Gemini output. The validator must enforce the relevant limits.

Difficulty should reflect reasoning complexity rather than simply adding more text.

## 8.4 Case consistency requirements

Every playable mystery must satisfy:

1. Exactly one designated culprit for a single-culprit case.
2. A complete and internally consistent timeline.
3. A coherent motive and opportunity.
4. Evidence that supports the actual solution.
5. At least two independent clues that materially support the culprit's identification.
6. At least one meaningful contradiction or deduction.
7. A plausible explanation for misleading evidence.
8. No unexplained contradictions in the canonical facts.
9. No required clue that is missing from the evidence collection.
10. No dependence on facts that were never presented to the user.

A misleading clue may be suspicious without being false. When an apparent contradiction exists, the solution must explain it.

The case should be rejected or repaired if the intended culprit cannot reasonably be distinguished from other suspects.

## 8.5 Evidence fairness

The correct accusation must be supported by evidence available before the accusation.

The final explanation must cite the specific evidence IDs that establish the solution.

The AI should not decide that an accusation is correct based on subjective writing quality, the user's explanation length, or whether their theory resembles the model's preferred phrasing.

---

# 9. GEMINI INTEGRATION

## 9.1 Configuration

Use the official Google GenAI SDK supported by the chosen Node.js environment.

Store configuration in environment variables:

- `GEMINI_API_KEY`
- `GEMINI_TEXT_MODEL`
- `MONGODB_URI`
- `PORT`
- `CLIENT_ORIGIN`
- `JWT_SECRET`, if authentication is implemented.
- `NODE_ENV`

Use a configurable Gemini model identifier rather than hardcoding a potentially outdated model.

Never expose `GEMINI_API_KEY` in React code, public environment variables, frontend bundles, URLs, logs, or API responses.

## 9.2 AI service responsibilities

Create a dedicated backend service, for example:

`server/src/services/gemini.service.js`

It should support:
- Canonical case generation.
- Timeline generation.
- Evidence generation.
- Consistency repair.
- Structured-output parsing.
- Bounded retry logic.
- Token and generation-error reporting.
- Model selection from configuration.

Use structured JSON output where supported.

Treat model output as untrusted input. Parse it, validate it, and reject invalid structures.

Do not trust the model to enforce security, uniqueness, access control, or spoiler protection.

## 9.3 Structured response format

The AI should generate data conforming to a versioned schema.

Illustrative canonical case:

{
  "schemaVersion": "1.0",
  "title": "The Last Train to Ashford",
  "summary": "A valuable artifact disappears during a private exhibition.",
  "difficulty": "medium",
  "category": "theft",
  "setting": "Ashford Central Station",
  "culpritId": "suspect_03",
  "motive": "A financial dispute",
  "method": "A staged transfer",
  "timeline": [],
  "suspects": [],
  "evidence": [],
  "solution": {
    "summary": "A complete explanation of the case.",
    "reasoning": [],
    "evidenceIds": []
  }
}

This example describes the internal server-side structure only. Do not return it directly to the browser.

Actual generation schemas must be defined in code and validated with a schema-validation library such as Zod.

## 9.4 Evidence-generation instructions

Every evidence-generation request must include a compact, authoritative representation of:
- The canonical case facts.
- Suspect identities.
- The established timeline.
- Known events.
- Permitted evidence types.
- Evidence IDs already assigned.
- Facts that must remain undisclosed.
- The exact evidence item being generated.

The model must not invent a new culprit, add an unapproved suspect, change a key event, or introduce a decisive fact inconsistent with the canonical case.

## 9.5 Validation and repair

For invalid model output:

1. Parse the response.
2. Validate against the schema.
3. Check references to suspect IDs and evidence IDs.
4. Check required fields and limits.
5. Check timeline and culprit consistency.
6. Request a bounded repair when possible.
7. Reject the generation if it remains invalid.
8. Return a friendly error to the user.

Do not enter an unlimited generation-repair loop.

## 9.6 Asset generation

Use a separate asset-generation abstraction:

`server/src/services/asset-generation.service.js`

This service should expose operations such as:
- Generate a fictional image.
- Store an image.
- Generate or obtain a video asset, if supported.
- Generate storyboard frames.
- Return asset metadata.

The initial implementation may use placeholders for unavailable media capabilities. Placeholders must be clearly labeled and must not break the investigation.

Gemini-generated text descriptions may be used to guide a separate image-generation service. Keep provider-specific implementation behind the service abstraction.

---

# 10. SPOILER PROTECTION

This is a critical architectural requirement.

The canonical case contains secret information, including:
- Culprit ID.
- Motive.
- Method.
- Hidden event details.
- Secret relationships.
- Full solution.
- Internal evidence interpretation.

Never return the full canonical case object to the frontend.

Implement separate DTOs (data transfer objects) for different states.

## Public investigation DTO

May include:
- Case ID.
- Title.
- Public summary.
- Difficulty.
- Public setting.
- Suspect profiles.
- Public timeline entries.
- Evidence metadata and content.
- User investigation progress.

Must not include:
- Culprit ID.
- Secret motive or method.
- Hidden solution.
- Internal AI prompts.
- Secret evaluation criteria.

## Result DTO

Available only after the user has submitted a valid final accusation, subject to the case's completion rules.

May include:
- Selected suspect.
- Whether the accusation was correct.
- Actual culprit.
- Full solution.
- Explanation of key deductions.
- Supporting evidence IDs.
- Final case status.

The backend must enforce spoiler rules on every request.

Frontend hiding, disabled buttons, CSS, and route guards are not security boundaries.

---

# 11. BACKEND ARCHITECTURE

Use a modular Express application.

Recommended directory structure:

server/
- src/
  - app.js
  - server.js
  - config/
    - env.js
    - database.js
  - routes/
    - case.routes.js
    - evidence.routes.js
    - accusation.routes.js
    - history.routes.js
    - auth.routes.js
  - controllers/
    - case.controller.js
    - evidence.controller.js
    - accusation.controller.js
    - history.controller.js
  - services/
    - gemini.service.js
    - case-generation.service.js
    - evidence.service.js
    - accusation.service.js
    - asset-generation.service.js
  - models/
    - Case.js
    - Investigation.js
    - User.js
  - validators/
    - case.schema.js
    - evidence.schema.js
    - accusation.schema.js
  - middleware/
    - error.middleware.js
    - rate-limit.middleware.js
    - auth.middleware.js
    - request-validation.middleware.js
  - utils/
    - ids.js
    - sanitization.js
    - spoiler-filter.js
  - tests/

Keep route handling, business logic, AI generation, database access, and validation separate.

Use consistent asynchronous error handling and a centralized error middleware.

---

# 12. FRONTEND ARCHITECTURE

Use React with Vite.

Recommended structure:

client/
- src/
  - app/
    - App.jsx
    - router.jsx
  - components/
    - layout/
    - navigation/
    - evidence/
    - suspects/
    - investigation/
    - forms/
    - feedback/
  - pages/
    - LandingPage.jsx
    - CreateCasePage.jsx
    - InvestigationPage.jsx
    - EvidencePage.jsx
    - AccusationPage.jsx
    - ResultPage.jsx
    - HistoryPage.jsx
    - NotFoundPage.jsx
  - services/
    - api.js
    - case.api.js
    - evidence.api.js
    - accusation.api.js
  - hooks/
  - context/
  - utils/
  - styles/
    - tokens.css
    - globals.css
  - tests/

Use React Router for navigation.

Use a lightweight state-management solution initially, such as React Context or Zustand if needed.

Keep server-owned case data in API responses and avoid storing the secret solution in browser state, local storage, session storage, or client-side caches.

Persist investigation notes and progress through authorized backend endpoints.

The frontend must display meaningful loading states during case generation and prevent duplicate submissions.

---

# 13. MONGODB DATA MODEL

Use MongoDB with Mongoose.

## 13.1 Case model

A case represents the generated mystery and its canonical solution.

Suggested fields:

- `_id`: MongoDB ObjectId.
- `schemaVersion`: String.
- `title`: String.
- `scenario`: String.
- `summary`: String.
- `difficulty`: Enum: easy, medium, hard.
- `category`: String.
- `setting`: String.
- `status`: generating, validating, ready, failed, archived.
- `suspects`: Array of structured suspect objects.
- `timeline`: Array of canonical timeline events.
- `evidence`: Array of structured evidence objects.
- `culpritId`: String, private.
- `motive`: String, private.
- `method`: String, private.
- `solution`: Structured private solution.
- `generationMetadata`: Model ID, generation timestamps, retry count, and schema version.
- `createdBy`: User ID or guest ownership identifier.
- `createdAt`: Date.
- `updatedAt`: Date.
- `expiresAt`: Optional date for guest-case retention.

Only ready cases may be played.

For larger cases, evidence and assets may be stored in separate collections to avoid document-size limitations.

## 13.2 Suspect structure

Each suspect must include:

- `id`: Stable string ID.
- `name`: String.
- `age`: Optional fictional age.
- `occupation`: String.
- `relationshipToCase`: String.
- `publicDescription`: String.
- `alibi`: Public or appropriately gated statement.
- `portraitAssetId`: Optional.
- `publicFacts`: Array.
- `privateFacts`: Internal facts, if needed.

Do not expose internal guilt labels in public suspect DTOs.

## 13.3 Evidence structure

Each evidence item must include:

- `id`: Stable string ID.
- `type`: message, image, cctv, witness, document, report.
- `title`: String.
- `description`: String.
- `content`: Type-specific structured object.
- `source`: String.
- `timestamp`: Optional date or case-relative timestamp.
- `relatedSuspectIds`: Array.
- `relatedLocationIds`: Array.
- `relatedEvidenceIds`: Array.
- `assetIds`: Array.
- `visibility`: Public or gated.
- `createdAt`: Date.

Do not depend on free-text evidence titles for identity or authorization. Use stable IDs.

## 13.4 Investigation model

An investigation represents a user's attempt to solve a case.

Fields:

- `_id`: ObjectId.
- `caseId`: Reference to Case.
- `userId`: Optional registered user reference.
- `guestSessionId`: Protected guest-session reference.
- `status`: active, completed, abandoned.
- `reviewedEvidenceIds`: Array.
- `investigationNotes`: String or structured note collection.
- `discoveredClueIds`: Array.
- `accusation`: Optional object.
- `result`: Optional result summary.
- `startedAt`: Date.
- `completedAt`: Optional date.
- `createdAt`: Date.
- `updatedAt`: Date.

Apply indexes for case ownership, user history, and relevant expiration queries.

## 13.5 User model

Only needed when authentication is implemented.

Fields:
- `_id`.
- `displayName`.
- `email`, unique and normalized.
- `passwordHash`, only if password authentication is used.
- `createdAt`.
- `updatedAt`.

Never store plaintext passwords.

## 13.6 Data ownership

A user must not be able to read or modify another user's private investigation through a guessed MongoDB ObjectId.

Guest sessions must use secure, unpredictable ownership credentials. Do not treat possession of a public case ID as proof of ownership.

Use server-side ownership checks for every private resource.

---

# 14. API CONTRACT

Base path: `/api/v1`

All responses use JSON unless a dedicated asset endpoint serves media.

## Standard success response

{
  "success": true,
  "data": {}
}

## Standard error response

{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "The supplied request is invalid."
  }
}

Never include stack traces, API keys, prompts, private culprit data, or internal model output in client-facing errors.

## 14.1 Create a mystery

`POST /api/v1/cases`

Request:

{
  "scenario": "A valuable painting disappears during a private exhibition.",
  "title": "",
  "difficulty": "medium",
  "category": "theft",
  "setting": "modern city",
  "evidencePreferences": [
    "message",
    "image",
    "cctv",
    "witness"
  ],
  "caseLength": "standard"
}

Response:

{
  "success": true,
  "data": {
    "case": {
      "id": "case_public_id",
      "title": "The Missing Painting",
      "summary": "A painting disappears during a private exhibition.",
      "difficulty": "medium",
      "status": "ready",
      "suspectCount": 4,
      "evidenceCount": 10
    }
  }
}

The response must contain only the public DTO.

For asynchronous generation, the endpoint may return `202 Accepted` with a public generation job ID. In that case, the frontend polls the generation-status endpoint until the case is ready or failed.

The implementation must choose one generation contract and use it consistently.

## 14.2 Get a case

`GET /api/v1/cases/:caseId`

Returns public case metadata, public suspects, and safe timeline information.

Never return the canonical secret fields.

## 14.3 Get case evidence

`GET /api/v1/cases/:caseId/evidence`

Returns the evidence list available to the authorized investigation.

Support optional filters such as `type` and `status`.

## 14.4 Get individual evidence

`GET /api/v1/cases/:caseId/evidence/:evidenceId`

Returns the selected evidence content and approved relationships.

Validate that both IDs belong to the authorized case and investigation.

## 14.5 Update investigation progress

`PATCH /api/v1/cases/:caseId/investigation`

Example request:

{
  "reviewedEvidenceIds": ["evidence_01"],
  "investigationNotes": "The timeline conflicts with the witness statement."
}

The backend must validate IDs, sanitize input, and enforce ownership.

## 14.6 Submit accusation

`POST /api/v1/cases/:caseId/accuse`

Request:

{
  "suspectId": "suspect_03"
}

The backend compares the selected suspect ID with the stored canonical culprit ID.

The first MVP version should permit one final accusation per investigation.

Response:

{
  "success": true,
  "data": {
    "correct": false,
    "selectedSuspectId": "suspect_03",
    "status": "completed",
    "resultAvailable": true
  }
}

This response example illustrates an incorrect accusation. The actual correctness value must be calculated from the stored case, never generated by Gemini.

The full solution must not be returned until the accusation is successfully recorded and the case completion rules are satisfied.

## 14.7 Get result

`GET /api/v1/cases/:caseId/result`

Returns the full solution only when the investigation is eligible to view it.

Validate ownership and completion status.

## 14.8 History

`GET /api/v1/history`

Returns the authenticated user's or authorized guest's investigation history.

## 14.9 Delete investigation

`DELETE /api/v1/cases/:caseId/investigation`

Deletes or resets the caller's investigation according to the retention policy.

Do not delete a shared case belonging to another user.

## 14.10 Health check

`GET /api/v1/health`

Returns service health and database connectivity status without exposing credentials or infrastructure secrets.

---

# 15. ACCUSATION AND SCORING LOGIC

The MVP must use deterministic answer checking.

When the user submits a suspect ID:

1. Validate the request.
2. Verify the case is ready.
3. Verify the investigation belongs to the caller.
4. Verify the investigation is still active.
5. Verify the suspect ID belongs to the case.
6. Compare the submitted suspect ID with the stored culprit ID.
7. Save the accusation and result.
8. Mark the investigation completed.
9. Return the result status.
10. Allow the eligible result endpoint to provide the full explanation.

Do not ask Gemini to decide whether the answer is correct.

Do not compare suspect names using fuzzy string matching. Use stable suspect IDs.

## Initial scoring

- Correct accusation: 100 points.
- Incorrect accusation: 0 points.
- Optional later bonuses: efficient evidence review, optional hint usage, and difficulty multipliers.

Scoring is secondary to delivering a fair investigation. Do not introduce complex scoring before the core workflow is stable.

---

# 16. SECURITY REQUIREMENTS

Implement the following protections:

1. Validate every request body, query parameter, and route parameter.
2. Use Helmet for secure HTTP headers.
3. Configure CORS for approved frontend origins.
4. Apply rate limits to case creation, generation-status polling, and accusation endpoints.
5. Keep Gemini credentials server-side.
6. Never expose secret case fields.
7. Enforce resource ownership.
8. Prevent NoSQL injection through strict schema validation.
9. Sanitize rendered user-generated content.
10. Avoid rendering arbitrary AI-generated HTML.
11. Do not execute AI-generated JavaScript.
12. Restrict remote asset URLs to approved storage or provider domains.
13. Limit request sizes and generated-content lengths.
14. Apply generation quotas to prevent cost abuse.
15. Redact secrets and sensitive case data from logs.
16. Use secure session or authentication practices.
17. Set retention rules for guest data.
18. Avoid exposing database connection details in error messages.
19. Keep dependencies maintained and use lockfiles.
20. Add security tests for spoiler leaks and cross-user access.

If cookie-based authentication is implemented, configure HttpOnly, Secure, and appropriate SameSite attributes, and protect state-changing requests against CSRF as needed.

Do not store session secrets in browser local storage.

AI prompts and user-supplied scenarios must be treated as untrusted input. The system must not follow requests embedded in scenarios that attempt to expose system instructions, alter application behavior, or override case validation rules.

---

# 17. PRIVACY AND CONTENT RULES

- Every mystery is fictional.
- Generated evidence must not be presented as genuine criminal evidence.
- Display a fictional-content disclaimer where appropriate.
- Do not imply that generated images show real suspects or victims.
- Avoid real private individuals as targets of fabricated criminal accusations.
- Do not accept uploaded content as proof of a real crime.
- Do not publish private investigations without explicit authorization.
- Do not generate graphic crime-scene imagery.
- Avoid instructions that enable real-world wrongdoing.
- Provide a clear mechanism for reporting abusive content if public sharing is introduced.

Recommended disclaimer:

"AI Mystery Room creates fictional investigations for entertainment. All characters, conversations, documents, images, and events are fictional or AI-generated unless explicitly stated otherwise."

---

# 18. ERROR HANDLING

Use stable error codes.

| Code | Meaning |
|---|---|
| `VALIDATION_ERROR` | Invalid request data |
| `CASE_NOT_FOUND` | Case does not exist or is inaccessible |
| `CASE_NOT_READY` | Generation has not completed |
| `GENERATION_FAILED` | AI generation failed |
| `GENERATION_TIMEOUT` | Generation exceeded its time budget |
| `INVALID_EVIDENCE_ID` | Evidence does not belong to the case |
| `INVALID_SUSPECT_ID` | Suspect does not belong to the case |
| `ALREADY_COMPLETED` | Investigation is already completed |
| `RESULT_NOT_AVAILABLE` | Result reveal is not yet allowed |
| `UNAUTHORIZED` | Authentication or session required |
| `FORBIDDEN` | Caller lacks permission |
| `RATE_LIMITED` | Request limit exceeded |
| `INTERNAL_ERROR` | Unexpected server error |

The frontend must provide a useful recovery action where possible.

For example, a failed generation should offer a retry without creating multiple accidental duplicate investigations.

Do not silently fall back to a random hardcoded mystery when Gemini fails. If a demo fallback is implemented, identify it clearly as demo content.

---

# 19. PERFORMANCE AND RELIABILITY

Initial targets:

- Landing page should feel responsive on a typical mobile connection.
- Cached public metadata should return quickly.
- Case generation should display progress rather than block the interface without feedback.
- Evidence navigation should not trigger unnecessary regeneration.
- MongoDB should store the generated case so the same case can be reopened without another AI call.
- Generation and asset creation should be bounded by configurable timeouts.
- Retry policies must be limited and observable.
- Failed or incomplete cases must not become playable.
- API pagination should be used for long history lists.
- Image assets should be appropriately sized and lazy-loaded.

Do not assume that AI generation can always complete within a fixed short response time.

If generation is asynchronous, use a persisted generation status and a polling interval with backoff. Polling must stop when the case is ready, fails, or the user leaves the workflow.

For production deployment, use a durable job queue if generation volume or task duration makes in-process background work unreliable.

---

# 20. TESTING REQUIREMENTS

## 20.1 Unit tests

Test:
- Request validation.
- Case schema validation.
- Evidence schema validation.
- Suspect ID validation.
- Deterministic accusation checking.
- Spoiler filtering.
- Ownership checks.
- Generation retry limits.
- Error formatting.

## 20.2 Integration tests

Test:
- MongoDB case creation.
- Successful generation pipeline using mocked Gemini responses.
- Invalid AI output rejection.
- Evidence retrieval.
- Investigation persistence.
- Correct accusation.
- Incorrect accusation.
- Duplicate accusation rejection.
- Unauthorized result access.
- Generation failure recovery.

## 20.3 End-to-end tests

Test the complete user flow:

1. Open the landing page.
2. Create a mystery.
3. Wait for generation to finish.
4. Enter the investigation.
5. Open at least one suspect.
6. Review multiple evidence types.
7. Submit an accusation.
8. View the result.
9. Verify the solution matches the canonical case.

## 20.4 Mandatory spoiler tests

For every public API response, verify that the following are absent:

- `culpritId`.
- Secret motive.
- Secret method.
- Full solution.
- Internal evaluation metadata.
- Hidden AI prompts.

Also test that guessing a case ID or evidence ID does not bypass ownership or result restrictions.

---

# 21. ACCEPTANCE CRITERIA

The MVP is complete only when all the following conditions are met.

### AC-01: Mystery creation
The user can submit a valid scenario and receive a generated case.

### AC-02: Consistent culprit
The case has one fixed culprit and a coherent canonical solution.

### AC-03: Playable evidence
The user can access evidence through the investigation interface.

### AC-04: Multiple evidence formats
The interface supports messages, images or labeled image placeholders, CCTV reports or available media, and witness statements.

### AC-05: Logical fairness
The final solution can be derived from information available to the player.

### AC-06: Stable investigation
Reloading the case does not regenerate or alter the canonical mystery.

### AC-07: Correct accusation
Selecting the stored culprit returns a correct result.

### AC-08: Incorrect accusation
Selecting another valid suspect returns an incorrect result.

### AC-09: Solution reveal
The full solution is available only after the authorized completion action.

### AC-10: Spoiler isolation
No public API exposes the culprit or secret solution before completion.

### AC-11: Persistence
Case data and investigation progress survive server restarts.

### AC-12: Failure recovery
Invalid model output or Gemini failures do not create playable broken cases.

### AC-13: Responsive UI
The main workflow works on mobile, tablet, and desktop.

### AC-14: Security
Unauthorized users cannot access private investigation records or reveal protected solutions.

### AC-15: Honest media
The UI never represents a placeholder as actual generated video or imagery.

### AC-16: Quality
The application has no dead-end primary actions, broken navigation, or unhandled core-flow errors.

---

# 22. IMPLEMENTATION PHASES

## Phase 1 — Foundation

- Initialize the MERN repository.
- Configure Vite and React.
- Configure Express.
- Connect MongoDB.
- Add environment validation.
- Implement centralized errors.
- Add API health check.
- Establish design tokens and routing.

**Deliverable:** Working application shell and database connection.

## Phase 2 — Core AI engine

- Integrate Gemini.
- Define canonical case schemas.
- Generate suspects and timeline.
- Generate evidence from canonical facts.
- Validate case consistency.
- Implement bounded repair and retry.
- Persist playable cases.

**Deliverable:** A valid mystery can be generated and stored.

## Phase 3 — Investigation experience

- Build landing page.
- Build creation form.
- Build investigation room.
- Build suspect cards.
- Build evidence viewer.
- Add message transcripts.
- Add image and CCTV presentation.
- Add investigation notes and progress.

**Deliverable:** A user can investigate a generated case.

## Phase 4 — Accusation and solution

- Implement accusation endpoint.
- Implement deterministic evaluation.
- Implement completion rules.
- Implement spoiler-safe result endpoint.
- Build result and solution pages.
- Persist case history.

**Deliverable:** A complete end-to-end game loop.

## Phase 5 — Reliability and security

- Add automated tests.
- Add rate limiting.
- Validate ownership and permissions.
- Audit every API response for secret fields.
- Test generation failures.
- Test mobile responsiveness.
- Add useful logging and generation metrics.

**Deliverable:** MVP ready for controlled deployment.

## Phase 6 — Optional enhancements

- User registration.
- Real AI-generated media.
- Hints and scoring.
- Daily mysteries.
- Achievements.
- Multiplayer features.

Implement these only after the MVP passes its acceptance criteria.

---

# 23. ENVIRONMENT AND DEVELOPER EXPERIENCE

Provide:

- `.env.example` with variable names and safe placeholders.
- `.gitignore` covering secrets, dependencies, generated assets, and build artifacts.
- Separate client and server scripts.
- Consistent linting and formatting.
- A README with setup and deployment instructions.
- Database initialization instructions.
- A documented AI provider configuration.
- A test command.
- A production build command.

Suggested root scripts:

- `npm run dev`
- `npm run dev:client`
- `npm run dev:server`
- `npm run build`
- `npm run test`
- `npm run lint`

Use a root workspace or equivalent scripts to coordinate the client and server.

Never commit `.env` files containing credentials.

---

# 24. CODEX EXECUTION RULES

Codex must treat this specification as the authoritative product and technical contract.

## Rules

1. Read `SPEC.md` before implementing any feature.
2. Inspect the existing repository before changing files.
3. Reuse established conventions when they do not conflict with this specification.
4. Do not replace the MERN stack without explicit approval.
5. Use Gemini for the mystery-generation engine.
6. Do not move AI API calls into the frontend.
7. Do not expose canonical solutions through public endpoints.
8. Do not fabricate integrations or claim unavailable media generation works.
9. Do not add unrelated features that delay the core investigation flow.
10. Validate all AI-generated output.
11. Implement deterministic accusation checking.
12. Add tests for every critical business rule.
13. Update this specification if an approved product decision changes.
14. Keep the README and `.env.example` synchronized with the implementation.
15. Never hardcode API keys, production credentials, or user secrets.
16. Do not mark a phase complete until its acceptance criteria pass.
17. If a requirement is ambiguous, choose the simplest secure implementation consistent with this specification and document the decision.
18. If an external dependency is unavailable, implement a clearly labeled abstraction or fallback instead of pretending the integration works.

## Definition of done

A feature is complete when:
- Its backend logic works.
- Its frontend workflow is functional.
- Its data is validated and persisted where required.
- Its access controls are enforced.
- Its error states are handled.
- Its relevant tests pass.
- Its UI is responsive.
- Its behavior matches this specification.

---

# 25. FINAL PRODUCT DEFINITION

AI Mystery Room is not simply an AI chatbot that invents a mystery and asks the user to guess a name.

It is a structured, stateful investigation game.

The canonical mystery is created first. Suspects, timelines, and evidence are derived from that canonical truth. The user explores evidence through an interactive interface, submits an accusation, and receives a deterministic result with a logically supported explanation.

The central architectural requirement is the separation of **canonical case truth**, **player-visible evidence**, and **post-investigation solution**.

All implementation decisions must preserve this separation.

**Success means a user can create a fictional mystery, investigate a coherent collection of AI-generated evidence, make a meaningful deduction, and discover a satisfying solution without encountering contradictions, broken media, or accidental spoilers.**
