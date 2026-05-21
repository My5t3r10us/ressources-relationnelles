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
  - generic [ref=e2]:
    - complementary [ref=e3]:
      - generic [ref=e4]:
        - generic [ref=e5]: A
        - generic [ref=e6]:
          - heading "PANNEAU ADMIN" [level=2] [ref=e7]
          - paragraph [ref=e8]: L'autorité sereine
      - link "Nouvelle ressource" [ref=e10] [cursor=pointer]:
        - /url: /publier
        - img [ref=e11]
        - text: Nouvelle ressource
      - navigation [ref=e14]:
        - link "Statistiques" [ref=e15] [cursor=pointer]:
          - /url: /admin/statistiques
          - img [ref=e16]
          - text: Statistiques
        - link "Modération" [ref=e21] [cursor=pointer]:
          - /url: /admin/moderation
          - img [ref=e22]
          - text: Modération
        - link "Signalements" [ref=e28] [cursor=pointer]:
          - /url: /admin/signalements
          - img [ref=e29]
          - text: Signalements
        - link "Ressources" [ref=e31] [cursor=pointer]:
          - /url: /admin/ressources
          - img [ref=e32]
          - text: Ressources
        - link "Catégories" [ref=e35] [cursor=pointer]:
          - /url: /admin/categories
          - img [ref=e36]
          - text: Catégories
        - link "Comptes utilisateurs" [ref=e39] [cursor=pointer]:
          - /url: /admin/utilisateurs
          - img [ref=e40]
          - text: Comptes utilisateurs
      - link "Support" [ref=e46] [cursor=pointer]:
        - /url: /aide
        - img [ref=e47]
        - text: Support
    - generic [ref=e51]:
      - banner [ref=e52]:
        - link "(RE)Sources Admin" [ref=e53] [cursor=pointer]:
          - /url: /admin/statistiques
        - generic [ref=e54]:
          - link "Retour à l'app" [ref=e55] [cursor=pointer]:
            - /url: /
            - img [ref=e56]
            - text: Retour à l'app
          - generic [ref=e59]: A
      - main [ref=e60]:
        - generic [ref=e61]:
          - heading "Gestion des catégories" [level=1] [ref=e62]
          - paragraph [ref=e63]: Créez, modifiez et supprimez les catégories de ressources de la plateforme.
          - generic [ref=e64]:
            - heading "Ajouter une catégorie" [level=2] [ref=e65]
            - paragraph [ref=e66]: Le nom et le slug sont requis.
            - generic [ref=e67]:
              - generic [ref=e68]:
                - generic [ref=e69]: Nom *
                - 'textbox "ex: Santé mentale" [ref=e70]'
              - generic [ref=e71]:
                - generic [ref=e72]: Slug *
                - 'textbox "ex: sante-mentale" [ref=e73]'
              - generic [ref=e74]:
                - generic [ref=e75]: Description
                - textbox "Description courte" [ref=e76]
              - generic [ref=e77]:
                - generic [ref=e78]: Icône
                - button "Choisir un émoji" [ref=e80]:
                  - img [ref=e81]
                  - generic [ref=e86]: Choisir un émoji
            - button "Créer la catégorie" [active] [ref=e87]
          - generic [ref=e89]:
            - generic [ref=e90]: 📁
            - generic [ref=e91]:
              - paragraph [ref=e92]: Bien-être
              - paragraph [ref=e93]: bien-etre
            - generic [ref=e94]: 1 ressource
            - generic [ref=e95]:
              - button [ref=e96]:
                - img [ref=e97]
              - button [ref=e100]:
                - img [ref=e101]
  - button "Open Next.js Dev Tools" [ref=e112] [cursor=pointer]:
    - img [ref=e113]
  - alert [ref=e117]
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