# GitHub Pages POS frontend

This frontend runs outside the Apps Script iframe so the browser can request camera permission for QR scanning. It calls the Apps Script backend through the Apps Script API with the signed-in user's OAuth access token.

## Required setup

1. In the Apps Script project, open **Project Settings** and associate it with the standard Google Cloud project `precise-duality-512605-r3`. The Apps Script project and OAuth client must use the same Cloud project.
2. In that Cloud project, enable the **Google Apps Script API** and configure the OAuth consent screen.
3. Create an OAuth client ID of type **Web application**. Add this authorized JavaScript origin:
   `https://sardartelecom2026-lang.github.io`
4. In `config.js`, replace:
   - `googleClientId` with the OAuth web client ID (never put a client secret in this frontend).
   - `appsScriptProjectId` with the **Script ID** from Apps Script > Project Settings. Do not use the `/exec` web-app URL or its deployment ID. The value supplied for this setup is already filled in; verify it matches the target script.
   - `oauthScopes`, if necessary, with every scope shown under Apps Script > Overview > Project OAuth scopes, plus the `script.projects` scope.
5. In Apps Script, deploy an **API executable** and restrict access to the intended signed-in Google account(s). The caller must also have access to the spreadsheet.
6. Copy the contents of this `github-pages` folder to the root of the `Sardartelecom` GitHub repository, enable GitHub Pages for the `main` branch root, and open:
   `https://sardartelecom2026-lang.github.io/Sardartelecom/`

The existing Apps Script `/exec` web-app deployment is separate and does not enable camera access inside its sandboxed iframe. The Apps Script API executable is the backend for this static frontend.

## Security notes

- The OAuth client ID is a public identifier; an OAuth client secret must never be placed in this site.
- Restrict the API executable and OAuth consent screen to the shop's authorized Google account(s). Do not publish an unrestricted owner-executing backend.
- The first sign-in must be initiated by the user. The browser will separately ask for camera permission when **Scan QR** is clicked.
