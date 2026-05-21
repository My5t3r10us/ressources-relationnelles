# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: admin.spec.ts >> Admin moderation >> admin can create a new category
- Location: tests\e2e\admin.spec.ts:43:7

# Error details

```
Test timeout of 60000ms exceeded.
```

```
Error: locator.fill: Test timeout of 60000ms exceeded.
Call log:
  - waiting for getByLabel(/nom/i).first()

```

# Page snapshot

```yaml
- generic [ref=e1]:
  - generic [ref=e3]:
    - banner [ref=e4]:
      - link "(RE)Sources Admin" [ref=e5] [cursor=pointer]:
        - /url: /admin/statistiques
      - generic [ref=e6]:
        - link "Retour à l'app" [ref=e7] [cursor=pointer]:
          - /url: /
          - img [ref=e8]
          - text: Retour à l'app
        - generic [ref=e10]: A
    - main [ref=e11]:
      - generic [ref=e12]:
        - heading "Gestion des catégories" [level=1] [ref=e13]
        - paragraph [ref=e14]: Créez, modifiez et supprimez les catégories de ressources de la plateforme.
        - generic [ref=e15]:
          - heading "Ajouter une catégorie" [level=2] [ref=e16]
          - paragraph [ref=e17]: Le nom et le slug sont requis.
          - generic [ref=e18]:
            - generic [ref=e19]:
              - generic [ref=e20]: Nom *
              - 'textbox "ex: Santé mentale" [ref=e21]'
            - generic [ref=e22]:
              - generic [ref=e23]: Slug *
              - 'textbox "ex: sante-mentale" [ref=e24]'
            - generic [ref=e25]:
              - generic [ref=e26]: Description
              - textbox "Description courte" [ref=e27]
            - generic [ref=e28]:
              - generic [ref=e29]: Icône
              - button "Choisir un émoji" [ref=e31]:
                - img [ref=e32]
                - generic [ref=e35]: Choisir un émoji
          - button "Créer la catégorie" [active] [ref=e36]
        - generic [ref=e38]:
          - generic [ref=e39]: 📁
          - generic [ref=e40]:
            - paragraph [ref=e41]: Bien-être
            - paragraph [ref=e42]: bien-etre
          - generic [ref=e43]: 1 ressource
          - generic [ref=e44]:
            - button [ref=e45]:
              - img [ref=e46]
            - button [ref=e49]:
              - img [ref=e50]
  - button "Open Next.js Dev Tools" [ref=e58] [cursor=pointer]:
    - img [ref=e59]
  - alert [ref=e62]
```

# Test source

```ts
  1  | import { test, expect } from "@playwright/test";
  2  | import { E2E_ADMIN, E2E_CITIZEN, login } from "./helpers/auth";
  3  | 
  4  | test.describe("Admin moderation", () => {
  5  |   test("citizen cannot access admin panel", async ({ page }) => {
  6  |     await login(page, E2E_CITIZEN);
  7  |     await page.goto("/fr/admin/statistiques");
  8  |     await expect(page).not.toHaveURL(/\/admin\/statistiques/, { timeout: 5_000 }).catch(async () => {
  9  |       await expect(page.getByText(/accès|forbidden|réservé/i)).toBeVisible();
  10 |     });
  11 |   });
  12 | 
  13 |   test("admin can access stats", async ({ page }) => {
  14 |     await login(page, E2E_ADMIN);
  15 |     await page.goto("/fr/admin/statistiques");
  16 |     await expect(page.getByRole("heading", { name: /statistiques/i })).toBeVisible();
  17 |   });
  18 | 
  19 |   test("admin can browse moderation page", async ({ page }) => {
  20 |     await login(page, E2E_ADMIN);
  21 |     await page.goto("/fr/admin/moderation");
  22 |     await expect(page.getByRole("heading", { name: /modération/i })).toBeVisible();
  23 |   });
  24 | 
  25 |   test("admin can browse categories management", async ({ page }) => {
  26 |     await login(page, E2E_ADMIN);
  27 |     await page.goto("/fr/admin/categories");
  28 |     await expect(page.getByRole("heading", { name: /catégories/i })).toBeVisible();
  29 |   });
  30 | 
  31 |   test("admin can browse users management", async ({ page }) => {
  32 |     await login(page, E2E_ADMIN);
  33 |     await page.goto("/fr/admin/utilisateurs");
  34 |     await expect(page.getByRole("heading", { name: /utilisateurs|comptes/i })).toBeVisible();
  35 |   });
  36 | 
  37 |   test("admin can browse reports/signalements", async ({ page }) => {
  38 |     await login(page, E2E_ADMIN);
  39 |     await page.goto("/fr/admin/signalements");
  40 |     await expect(page.getByRole("heading", { name: /signalements|signalés/i })).toBeVisible();
  41 |   });
  42 | 
  43 |   test("admin can create a new category", async ({ page }) => {
  44 |     await login(page, E2E_ADMIN);
  45 |     await page.goto("/fr/admin/categories");
  46 |     const newButton = page.getByRole("button", { name: /nouvelle|créer|ajouter/i }).first();
  47 |     if (await newButton.isVisible().catch(() => false)) {
  48 |       await newButton.click();
  49 |       const slug = `e2e-${Date.now()}`;
  50 |       const nameField = page.getByLabel(/nom/i).first();
  51 |       const slugField = page.getByLabel(/slug/i).first();
> 52 |       await nameField.fill(`Cat ${slug}`);
     |                       ^ Error: locator.fill: Test timeout of 60000ms exceeded.
  53 |       await slugField.fill(slug);
  54 |       await page.getByRole("button", { name: /enregistrer|créer|valider/i }).first().click();
  55 |       await expect(page.getByText(slug)).toBeVisible({ timeout: 10_000 });
  56 |     }
  57 |   });
  58 | });
  59 | 
```