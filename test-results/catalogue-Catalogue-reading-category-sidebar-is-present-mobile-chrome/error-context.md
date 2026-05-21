# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: catalogue.spec.ts >> Catalogue & reading >> category sidebar is present
- Location: tests\e2e\catalogue.spec.ts:29:7

# Error details

```
Error: expect(locator).toBeVisible() failed

Locator:  getByText(/Toutes les ressources/i).first()
Expected: visible
Received: hidden
Timeout:  10000ms

Call log:
  - Expect "toBeVisible" with timeout 10000ms
  - waiting for getByText(/Toutes les ressources/i).first()
    23 × locator resolved to <span class="text-sm">Toutes les ressources</span>
       - unexpected value "hidden"

```

```yaml
- banner:
  - navigation:
    - link "(RE)Sources Relationnelles":
      - /url: /
    - button "Langue": fr
    - button "Aide":
      - link:
        - /url: /aide
    - link "Se connecter":
      - /url: /login
- main:
  - heading "Explorer les ressources" [level=1]
  - paragraph: Contenus sélectionnés pour soutenir votre bien-être mental, vos relations et votre vie professionnelle. Guidés par des experts, disponibles pour vous.
  - textbox "Rechercher par condition, sujet ou auteur..."
  - button "Vidéo"
  - button "Document"
  - button "Exercice"
  - button "Article"
  - paragraph:
    - text: Affichage de
    - strong: 1 résultat
  - text: "Trier par :"
  - combobox:
    - option "Plus récents" [selected]
    - option "Plus populaires"
    - option "Plus anciens"
  - link "À la une Article Ressource E2E publiée Résumé de test 1 min Commencer":
    - /url: /ressource/807f9328-2900-42ee-8ce3-7cea554db230
    - text: À la une Article
    - heading "Ressource E2E publiée" [level=3]
    - paragraph: Résumé de test
    - text: 1 min Commencer
- contentinfo:
  - paragraph: © 2024 (RE)Sources Relationnelles. Initiative officielle de santé publique.
  - link "Mentions légales":
    - /url: /mentions-legales
  - link "Accessibilité":
    - /url: /accessibilite
  - link "Confidentialité":
    - /url: /confidentialite
  - link "Contact Support":
    - /url: /contact
- alert
```

# Test source

```ts
  1  | import { test, expect } from "@playwright/test";
  2  | 
  3  | test.describe("Catalogue & reading", () => {
  4  |   test("catalogue page lists resources", async ({ page }) => {
  5  |     await page.goto("/fr/catalogue");
  6  |     await expect(page.getByRole("heading", { name: /catalogue/i })).toBeVisible();
  7  |     const cards = page.locator("article, [data-testid='resource-card'], a:has-text('Ressource E2E')");
  8  |     await expect(cards.first()).toBeVisible({ timeout: 10_000 });
  9  |   });
  10 | 
  11 |   test("search filters resources", async ({ page }) => {
  12 |     await page.goto("/fr/catalogue");
  13 |     const search = page.getByPlaceholder(/rechercher/i).first();
  14 |     if (await search.isVisible().catch(() => false)) {
  15 |       await search.fill("E2E");
  16 |       await search.press("Enter");
  17 |       await expect(page.getByText(/Ressource E2E publiée/i)).toBeVisible({ timeout: 10_000 });
  18 |     }
  19 |   });
  20 | 
  21 |   test("can navigate to a resource detail page", async ({ page }) => {
  22 |     await page.goto("/fr/catalogue");
  23 |     const firstResource = page.getByText(/Ressource E2E publiée/i).first();
  24 |     await firstResource.click();
  25 |     await expect(page).toHaveURL(/\/ressource\//);
  26 |     await expect(page.getByRole("heading", { name: /Ressource E2E publiée/i })).toBeVisible();
  27 |   });
  28 | 
  29 |   test("category sidebar is present", async ({ page }) => {
  30 |     await page.goto("/fr/catalogue");
> 31 |     await expect(page.getByText(/Toutes les ressources/i).first()).toBeVisible();
     |                                                                    ^ Error: expect(locator).toBeVisible() failed
  32 |   });
  33 | });
  34 | 
```