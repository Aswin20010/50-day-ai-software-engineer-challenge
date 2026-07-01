# Day 31 — API Integration Testing Checklist

## Purpose

This checklist validates the Day 31 frontend API integration foundation.

Day 31 introduced reusable API integration planning, shared TypeScript response types, reusable API client helpers, response display components, and environment-based backend configuration.

The goal of this checklist is to confirm that the UI still works with the existing Day 27–30 flow while also preparing the project for future external backend integration.

---

## Testing Scope

This checklist covers:

* Project structure validation
* Environment configuration
* TypeScript compilation
* Existing internal API route behavior
* New reusable API client behavior
* New response component compilation
* Build verification
* Manual UI smoke testing

---

## 1. File Structure Validation

Confirm the following files exist:

```text
src/types/factory.ts
src/lib/apiClient.ts
src/lib/env.ts
src/components/ErrorMessage.tsx
src/components/FactoryOutput.tsx
```

Confirm the existing page still exists:

```text
src/app/page.tsx
```

Expected result:

```text
All required Day 31 files are present.
```

---

## 2. Existing Page Preservation Check

Open:

```text
src/app/page.tsx
```

Confirm the file still includes the existing Day 27–30 functionality:

* Requirement file upload
* Extract Requirements from Confluence button
* Normalized Extraction Review section
* Missing fields review section
* Generate Flow button
* Generation Metadata section
* Generated Flow Inspection section
* Download JSON button

Expected result:

```text
The existing page was not replaced by the simple Day 31 sample page.
```

---

## 3. Environment File Check

Confirm `.env.local` exists in the project root.

Expected file:

```env
NEXT_PUBLIC_API_BASE_URL=http://localhost:8080
```

Expected result:

```text
NEXT_PUBLIC_API_BASE_URL is configured.
```

Important note:

```text
After changing .env.local, restart the Next.js dev server.
```

---

## 4. Internal API Route Safety Check

Open:

```text
src/app/page.tsx
```

Confirm these internal API calls still use relative paths:

```ts
fetch("/api/extract-requirements", ...)
fetch("/api/generate", ...)
```

Expected result:

```text
Existing Next.js internal API routes still use relative paths and are not forced through NEXT_PUBLIC_API_BASE_URL.
```

Reason:

```text
/api/extract-requirements and /api/generate are internal Next.js API routes. They should stay relative unless they are moved to an external backend service later.
```

---

## 5. TypeScript Import Check

Confirm the new files use the correct alias imports:

```ts
import type { RequirementAnalyzeResponse } from "@/types/factory";
import { getExternalApiBaseUrl } from "@/lib/env";
```

Also confirm `tsconfig.json` supports the alias:

```json
{
  "compilerOptions": {
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

Expected result:

```text
No import alias errors occur during build.
```

---

## 6. Build Validation

Run:

```bash
npm run build
```

Expected result:

```text
Build completes successfully without TypeScript or lint errors.
```

If build fails, check:

* Missing imports
* Incorrect file paths
* Type mismatch in `RequirementAnalyzeResponse`
* Missing `@/*` path alias
* Incorrect component export names
* Invalid JSX syntax

---

## 7. Dev Server Validation

Start the frontend:

```bash
npm run dev
```

Open the app in the browser.

Expected result:

```text
The API Factory UI loads successfully.
```

Confirm there are no browser console errors related to:

* Missing environment variables
* Module not found
* Hydration errors
* Invalid component imports
* Failed page rendering

---

## 8. Existing Requirement Extraction Smoke Test

In the UI:

1. Upload a `.txt`, `.md`, or `.html` Confluence export file.
2. Click `Extract Requirements from Confluence`.
3. Wait for the extraction response.

Expected result:

```text
The extraction flow still works exactly as before Day 31 changes.
```

Confirm the UI can still display:

* Extract status
* Missing fields
* Normalized extraction review
* Requirement ID
* API name
* Endpoint
* Schema version
* Open questions
* Gaps
* Normalized JSON

---

## 9. Existing Flow Generation Smoke Test

After extraction:

1. Review the generated rules.
2. Click `Generate Flow`.
3. Review the generated JSON output.

Expected result:

```text
The flow generation still works and produces generated-flow JSON.
```

Confirm the UI can still display:

* Generated flow JSON
* Generation metadata
* Generation source
* Rule count
* SOAP subflow status
* Generated rule IDs
* Adapter warnings
* Full generation metadata JSON

---

## 10. Download JSON Test

After generating a flow:

1. Click `Download JSON`.
2. Confirm a file downloads.

Expected filename:

```text
generated-flow.json
```

Expected result:

```text
The generated Node-RED flow can still be downloaded.
```

---

## 11. New Environment Helper Validation

Open:

```text
src/lib/env.ts
```

Confirm it has:

```ts
export function getExternalApiBaseUrl(): string
export function getInternalApiPath(path: string): string
```

Expected result:

```text
External backend URLs and internal route paths are handled separately.
```

---

## 12. New API Client Validation

Open:

```text
src/lib/apiClient.ts
```

Confirm it includes:

```ts
analyzeRequirement(payload)
```

Confirm it posts to:

```text
/api/factory/requirements/analyze
```

Expected result:

```text
The reusable API client is ready for future external backend analysis calls.
```

Note:

```text
This client may not be used by the current page yet. That is acceptable for Day 31 because the existing page already uses internal API routes for extraction and generation.
```

---

## 13. New Component Compilation Check

Confirm these components compile successfully:

```text
src/components/ErrorMessage.tsx
src/components/FactoryOutput.tsx
```

Expected result:

```text
The reusable response components are available for future UI integration.
```

Note:

```text
These components do not need to be connected to the current page yet because the current page already has custom response rendering for extraction, generation, metadata, warnings, and flow output.
```

---

## 14. Regression Check

Confirm the following features were not broken:

| Feature                  | Expected Result                          |
| ------------------------ | ---------------------------------------- |
| File upload              | User can select a requirement file       |
| Requirement extraction   | Backend extraction API is called         |
| Missing fields           | Missing fields render when returned      |
| Normalized schema review | Normalized extraction displays correctly |
| Manual rule editing      | Rule forms can still be edited           |
| Add Nodes buttons        | Buttons still add correct node forms     |
| Remove button            | Rule can still be removed                |
| Generate Flow            | Flow generation API is called            |
| Generated output         | JSON output appears in textarea          |
| Metadata display         | Generation metadata still renders        |
| Download JSON            | Generated JSON downloads correctly       |

---

## 15. Common Issues and Fixes

### Issue: `NEXT_PUBLIC_API_BASE_URL is not configured`

Fix:

```text
Create .env.local in the project root and restart npm run dev.
```

---

### Issue: `Module not found: Can't resolve '@/types/factory'`

Fix:

```text
Confirm src/types/factory.ts exists and tsconfig.json has the @/* alias.
```

---

### Issue: Internal routes fail after adding API client

Fix:

```text
Do not route /api/extract-requirements or /api/generate through NEXT_PUBLIC_API_BASE_URL yet.
Keep them as relative Next.js API routes.
```

---

### Issue: Build fails because components are unused

Fix:

```text
Unused exported components are okay in Next.js. If a lint rule blocks unused files, import them later when integrating the new manual requirement analysis page.
```

---

### Issue: `fetch failed` for external analysis endpoint

Fix:

```text
Confirm the external backend is running on the URL configured in NEXT_PUBLIC_API_BASE_URL.
```

---

## Day 31 Completion Criteria

Day 31 Phase 6 is complete when:

* `npm run build` passes.
* Existing Day 27–30 UI functionality still works.
* Internal API routes remain relative.
* `.env.local` is configured.
* `src/lib/env.ts` exists.
* `src/lib/apiClient.ts` uses the environment helper.
* `FactoryOutput` and `ErrorMessage` components compile.
* Requirement extraction still works.
* Flow generation still works.
* Generated JSON download still works.

---

## Final Validation Command

Run:

```bash
npm run build
```

Then run:

```bash
npm run dev
```

Perform one full UI smoke test:

```text
Upload requirement file → extract requirements → review normalized output → generate flow → download JSON
```

If all steps pass, Day 31 API integration foundation is validated.
