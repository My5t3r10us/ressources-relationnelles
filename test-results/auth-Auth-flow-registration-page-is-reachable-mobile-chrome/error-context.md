# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: auth.spec.ts >> Auth flow >> registration page is reachable
- Location: tests\e2e\auth.spec.ts:24:7

# Error details

```
Error: expect(locator).toBeVisible() failed

Locator: getByRole('button', { name: /inscription|register|créer/i })
Expected: visible
Timeout: 10000ms
Error: element(s) not found

Call log:
  - Expect "toBeVisible" with timeout 10000ms
  - waiting for getByRole('button', { name: /inscription|register|créer/i })

```

```yaml
- heading "Créer un compte" [level=1]
- paragraph: Inscrivez-vous pour commencer
- text: Prénom
- textbox "Prénom":
  - /placeholder: Jean
- text: Nom
- textbox "Nom":
  - /placeholder: Dupont
- text: Email
- textbox "Email":
  - /placeholder: vous@exemple.com
- text: Mot de passe
- textbox "Mot de passe":
  - /placeholder: ••••••••
- text: Confirmer le mot de passe
- textbox "Confirmer le mot de passe":
  - /placeholder: ••••••••
- button "S'inscrire"
- paragraph:
  - text: Déjà un compte ?
  - link "Se connecter":
    - /url: /login
- alert
```

# Test source

```ts
  1  | import { test, expect } from "@playwright/test";
  2  | import { E2E_CITIZEN, login } from "./helpers/auth";
  3  | 
  4  | test.describe("Auth flow", () => {
  5  |   test("login page is reachable", async ({ page }) => {
  6  |     await page.goto("/fr/login");
  7  |     await expect(page.getByLabel(/email/i)).toBeVisible();
  8  |     await expect(page.getByLabel(/mot de passe/i)).toBeVisible();
  9  |   });
  10 | 
  11 |   test("wrong credentials show an error", async ({ page }) => {
  12 |     await page.goto("/fr/login");
  13 |     await page.getByLabel(/email/i).fill("nobody@nope.test");
  14 |     await page.getByLabel(/mot de passe/i).fill("wrong-password");
  15 |     await page.getByRole("button", { name: /se connecter|login/i }).click();
  16 |     await expect(page.getByText(/identifiants|invalid|incorrect/i)).toBeVisible({ timeout: 10_000 });
  17 |   });
  18 | 
  19 |   test("happy path: existing user logs in and reaches home", async ({ page }) => {
  20 |     await login(page, E2E_CITIZEN);
  21 |     await expect(page).not.toHaveURL(/\/login$/);
  22 |   });
  23 | 
  24 |   test("registration page is reachable", async ({ page }) => {
  25 |     await page.goto("/fr/register");
> 26 |     await expect(page.getByRole("button", { name: /inscription|register|créer/i })).toBeVisible();
     |                                                                                     ^ Error: expect(locator).toBeVisible() failed
  27 |   });
  28 | 
  29 |   test("registers a new user", async ({ page }) => {
  30 |     const stamp = Date.now();
  31 |     await page.goto("/fr/register");
  32 | 
  33 |     await page.getByLabel(/nom complet|nom/i).fill(`User${stamp}`);
  34 |     await page.getByLabel(/email/i).fill(`u${stamp}@test.local`);
  35 |     await page.getByLabel(/^mot de passe$/i).fill("Password123!");
  36 |     const confirmField = page.getByLabel(/confirmer|confirmation/i);
  37 |     if (await confirmField.isVisible().catch(() => false)) {
  38 |       await confirmField.fill("Password123!");
  39 |     }
  40 | 
  41 |     await page.getByRole("button", { name: /inscription|register|créer/i }).click();
  42 |     await expect(page).not.toHaveURL(/\/register$/, { timeout: 15_000 });
  43 |   });
  44 | });
  45 | 
```