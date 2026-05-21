# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: publish.spec.ts >> Publish a resource >> publish form validates required fields
- Location: tests\e2e\publish.spec.ts:14:7

# Error details

```
Error: expect(locator).toBeVisible() failed

Locator: getByText(/requis|obligatoire|nécessaire|title|content/i).first()
Expected: visible
Timeout: 10000ms
Error: element(s) not found

Call log:
  - Expect "toBeVisible" with timeout 10000ms
  - waiting for getByText(/requis|obligatoire|nécessaire|title|content/i).first()

```

```yaml
- banner:
  - navigation:
    - link "(RE)Sources Relationnelles":
      - /url: /
    - link "Catalogue":
      - /url: /catalogue
    - link "Bien-être":
      - /url: /bien-etre
    - link "Communauté":
      - /url: /communaute
    - link "Urgence":
      - /url: /urgence
    - button "Rechercher": Rechercher...
    - button "Langue": fr
    - button "Aide":
      - link:
        - /url: /aide
    - button "Mon compte": C
- link "Quitter le mode rédaction":
  - /url: /catalogue
- text: Brouillon...
- heading "Ressource sans titre" [level=1]
- paragraph: Commencez à partager vos connaissances. Le monde vous écoute.
- textbox "Donnez un titre à votre ressource..."
- button "H1"
- button "H2"
- button "H3"
- button "G":
  - strong: G
- button "I":
  - emphasis: I
- button "S"
- button "<>"
- button "≡"
- button "1≡"
- button "❝"
- 'button "{ }"'
- button "―"
- button "↩"
- button "↪"
- paragraph: Commencez à rédiger votre ressource ici...
- separator
- text: Catégorie
- combobox:
  - option "Choisir un chemin..." [selected]
  - option "Anxiété & Stress"
  - option "Équilibre vie pro/perso"
  - option "Parentalité"
  - option "Soutien de crise"
  - option "Santé mentale"
- text: Visibilité
- radio "Public— Visible par toute la communauté" [checked]
- text: Public— Visible par toute la communauté
- radio "Privé— Accessible uniquement par vous"
- text: Privé— Accessible uniquement par vous Format
- combobox:
  - option "Article / Réflexion" [selected]
  - option "Vidéo"
  - option "Document PDF"
  - option "Exercice"
  - option "Audio / Podcast"
  - option "Protocole"
- text: Image de couverture
- button "Ajouter une image de couverture JPG, PNG, WebP — recommandé 1200×400 px"
- button "Choose File"
- paragraph: Joignez des documents, vidéos ou fichiers audio
- button "Parcourir les fichiers"
- button "Choose File"
- button "Publier sur (RE)Sources"
- button "Enregistrer comme brouillon"
- contentinfo:
  - paragraph: © 2024 (RE)Sources Relationnelles. Une initiative officielle de santé publique.
  - button "Confidentialité"
  - button "Accessibilité"
  - button "Assistance"
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
  2  | import { E2E_CITIZEN, login } from "./helpers/auth";
  3  | 
  4  | test.describe("Publish a resource", () => {
  5  |   test.beforeEach(async ({ page }) => {
  6  |     await login(page, E2E_CITIZEN);
  7  |   });
  8  | 
  9  |   test("user can reach the publish page when authenticated", async ({ page }) => {
  10 |     await page.goto("/fr/publier");
  11 |     await expect(page.getByRole("heading", { name: /publier|nouvelle ressource/i })).toBeVisible();
  12 |   });
  13 | 
  14 |   test("publish form validates required fields", async ({ page }) => {
  15 |     await page.goto("/fr/publier");
  16 |     const submit = page.getByRole("button", { name: /publier|envoyer|soumettre/i }).first();
  17 |     await submit.click();
  18 |     await expect(
  19 |       page.getByText(/requis|obligatoire|nécessaire|title|content/i).first(),
> 20 |     ).toBeVisible({ timeout: 10_000 });
     |       ^ Error: expect(locator).toBeVisible() failed
  21 |   });
  22 | 
  23 |   test("user can submit a new resource", async ({ page }) => {
  24 |     await page.goto("/fr/publier");
  25 |     const title = `Test E2E ${Date.now()}`;
  26 |     await page.getByLabel(/titre/i).fill(title);
  27 | 
  28 |     const contentField = page.getByLabel(/contenu|description/i).first();
  29 |     if (await contentField.isVisible().catch(() => false)) {
  30 |       await contentField.fill(
  31 |         "Voici un contenu de test pour la publication automatisée d'une ressource",
  32 |       );
  33 |     }
  34 | 
  35 |     const summary = page.getByLabel(/résumé/i).first();
  36 |     if (await summary.isVisible().catch(() => false)) {
  37 |       await summary.fill("Résumé test E2E");
  38 |     }
  39 | 
  40 |     await page.getByRole("button", { name: /publier|soumettre/i }).first().click();
  41 |     await expect(page).not.toHaveURL(/\/publier$/, { timeout: 15_000 });
  42 |   });
  43 | 
  44 |   test("can list my own resources in /mes-ressources", async ({ page }) => {
  45 |     await page.goto("/fr/mes-ressources");
  46 |     await expect(page.getByRole("heading", { name: /mes ressources/i })).toBeVisible();
  47 |   });
  48 | });
  49 | 
```