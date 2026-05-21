# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: auth.spec.ts >> Auth flow >> registers a new user
- Location: tests\e2e\auth.spec.ts:29:7

# Error details

```
Error: locator.fill: Error: strict mode violation: getByLabel(/nom complet|nom/i) resolved to 2 elements:
    1) <input value="" type="text" required="" id="firstName" placeholder="Jean" class="block w-full bg-surface-container-high border-none rounded-xl px-4 py-3 text-on-surface placeholder:text-outline focus:ring-2 focus:ring-primary/20 focus:bg-surface-container-lowest focus:outline-none"/> aka getByRole('textbox', { name: 'Prénom' })
    2) <input value="" type="text" required="" id="lastName" placeholder="Dupont" class="block w-full bg-surface-container-high border-none rounded-xl px-4 py-3 text-on-surface placeholder:text-outline focus:ring-2 focus:ring-primary/20 focus:bg-surface-container-lowest focus:outline-none"/> aka getByRole('textbox', { name: 'Nom', exact: true })

Call log:
  - waiting for getByLabel(/nom complet|nom/i)

```

# Page snapshot

```yaml
- generic [active] [ref=e1]:
  - generic [ref=e3]:
    - generic [ref=e4]:
      - heading "Créer un compte" [level=1] [ref=e5]
      - paragraph [ref=e6]: Inscrivez-vous pour commencer
    - generic [ref=e7]:
      - generic [ref=e8]:
        - generic [ref=e9]:
          - generic [ref=e10]: Prénom
          - textbox "Prénom" [ref=e11]:
            - /placeholder: Jean
        - generic [ref=e12]:
          - generic [ref=e13]: Nom
          - textbox "Nom" [ref=e14]:
            - /placeholder: Dupont
      - generic [ref=e15]:
        - generic [ref=e16]: Email
        - textbox "Email" [ref=e17]:
          - /placeholder: vous@exemple.com
      - generic [ref=e18]:
        - generic [ref=e19]: Mot de passe
        - textbox "Mot de passe" [ref=e20]:
          - /placeholder: ••••••••
      - generic [ref=e21]:
        - generic [ref=e22]: Confirmer le mot de passe
        - textbox "Confirmer le mot de passe" [ref=e23]:
          - /placeholder: ••••••••
      - button "S'inscrire" [ref=e24]
    - paragraph [ref=e25]:
      - text: Déjà un compte ?
      - link "Se connecter" [ref=e26] [cursor=pointer]:
        - /url: /login
  - button "Open Next.js Dev Tools" [ref=e32] [cursor=pointer]:
    - img [ref=e33]
  - alert [ref=e37]
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
  26 |     await expect(page.getByRole("button", { name: /inscription|register|créer/i })).toBeVisible();
  27 |   });
  28 | 
  29 |   test("registers a new user", async ({ page }) => {
  30 |     const stamp = Date.now();
  31 |     await page.goto("/fr/register");
  32 | 
> 33 |     await page.getByLabel(/nom complet|nom/i).fill(`User${stamp}`);
     |                                               ^ Error: locator.fill: Error: strict mode violation: getByLabel(/nom complet|nom/i) resolved to 2 elements:
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