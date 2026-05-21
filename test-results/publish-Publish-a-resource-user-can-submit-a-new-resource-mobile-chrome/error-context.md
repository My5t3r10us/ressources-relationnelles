# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: publish.spec.ts >> Publish a resource >> user can submit a new resource
- Location: tests\e2e\publish.spec.ts:23:7

# Error details

```
Test timeout of 60000ms exceeded.
```

```
Error: locator.fill: Test timeout of 60000ms exceeded.
Call log:
  - waiting for getByLabel(/titre/i)

```

# Page snapshot

```yaml
- generic [active] [ref=e1]:
  - banner [ref=e2]:
    - navigation [ref=e3]:
      - link "(RE)Sources Relationnelles" [ref=e4] [cursor=pointer]:
        - /url: /
      - generic [ref=e5]:
        - button "Langue" [ref=e7]:
          - img [ref=e8]
          - generic [ref=e11]: fr
        - button "Aide" [ref=e12]:
          - link [ref=e13] [cursor=pointer]:
            - /url: /aide
            - img [ref=e14]
        - button "Mon compte" [ref=e18]: C
  - generic [ref=e19]:
    - generic [ref=e20]:
      - link "Quitter le mode rédaction" [ref=e21] [cursor=pointer]:
        - /url: /catalogue
        - img [ref=e22]
        - text: Quitter le mode rédaction
      - generic [ref=e25]: Brouillon...
    - generic [ref=e26]:
      - generic:
        - heading "Ressource sans titre" [level=1]
        - paragraph: Commencez à partager vos connaissances. Le monde vous écoute.
      - textbox "Donnez un titre à votre ressource..." [ref=e28]
      - generic [ref=e30]:
        - generic [ref=e31]:
          - button "H1" [ref=e32]
          - button "H2" [ref=e33]
          - button "H3" [ref=e34]
          - button "G" [ref=e36]:
            - strong [ref=e37]: G
          - button "I" [ref=e38]:
            - emphasis [ref=e39]: I
          - button "S" [ref=e40]
          - button "<>" [ref=e41]
          - button "≡" [ref=e43]
          - button "1≡" [ref=e44]
          - button "❝" [ref=e46]
          - 'button "{ }" [ref=e47]'
          - button "―" [ref=e48]
          - button "↩" [ref=e50]
          - button "↪" [ref=e51]
        - paragraph [ref=e54]: Commencez à rédiger votre ressource ici...
      - separator [ref=e55]
      - generic [ref=e56]:
        - generic [ref=e57]:
          - generic [ref=e58]: Catégorie
          - combobox [ref=e59] [cursor=pointer]:
            - option "Choisir un chemin..." [selected]
            - option "Anxiété & Stress"
            - option "Équilibre vie pro/perso"
            - option "Parentalité"
            - option "Soutien de crise"
            - option "Santé mentale"
        - generic [ref=e60]:
          - generic [ref=e61]: Visibilité
          - generic [ref=e62]:
            - generic [ref=e63] [cursor=pointer]:
              - radio "Public— Visible par toute la communauté" [checked] [ref=e65]
              - generic [ref=e68]:
                - text: Public
                - generic [ref=e69]: — Visible par toute la communauté
            - generic [ref=e70] [cursor=pointer]:
              - radio "Privé— Accessible uniquement par vous" [ref=e72]
              - generic [ref=e74]:
                - text: Privé
                - generic [ref=e75]: — Accessible uniquement par vous
      - generic [ref=e76]:
        - generic [ref=e77]: Format
        - combobox [ref=e78] [cursor=pointer]:
          - option "Article / Réflexion" [selected]
          - option "Vidéo"
          - option "Document PDF"
          - option "Exercice"
          - option "Audio / Podcast"
          - option "Protocole"
      - generic [ref=e79]:
        - generic [ref=e80]: Image de couverture
        - button "Ajouter une image de couverture JPG, PNG, WebP — recommandé 1200×400 px" [ref=e81]:
          - img [ref=e82]
          - generic [ref=e86]: Ajouter une image de couverture
          - generic [ref=e87]: JPG, PNG, WebP — recommandé 1200×400 px
        - button "Choose File" [ref=e88]
      - generic [ref=e89]:
        - img [ref=e90]
        - paragraph [ref=e92]: Joignez des documents, vidéos ou fichiers audio
        - button "Parcourir les fichiers" [ref=e93]
        - button "Choose File" [ref=e94]
      - generic [ref=e95]:
        - button "Publier sur (RE)Sources" [ref=e96]
        - button "Enregistrer comme brouillon" [ref=e97]
    - contentinfo [ref=e98]:
      - generic [ref=e99]:
        - paragraph [ref=e100]: © 2024 (RE)Sources Relationnelles. Une initiative officielle de santé publique.
        - generic [ref=e101]:
          - button "Confidentialité" [ref=e102]
          - button "Accessibilité" [ref=e103]
          - button "Assistance" [ref=e104]
  - contentinfo [ref=e105]:
    - generic [ref=e106]:
      - paragraph [ref=e107]: © 2024 (RE)Sources Relationnelles. Initiative officielle de santé publique.
      - generic [ref=e108]:
        - link "Mentions légales" [ref=e109] [cursor=pointer]:
          - /url: /mentions-legales
        - link "Accessibilité" [ref=e110] [cursor=pointer]:
          - /url: /accessibilite
        - link "Confidentialité" [ref=e111] [cursor=pointer]:
          - /url: /confidentialite
        - link "Contact Support" [ref=e112] [cursor=pointer]:
          - /url: /contact
  - button "Open Next.js Dev Tools" [ref=e118] [cursor=pointer]:
    - img [ref=e119]
  - alert [ref=e122]
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
  20 |     ).toBeVisible({ timeout: 10_000 });
  21 |   });
  22 | 
  23 |   test("user can submit a new resource", async ({ page }) => {
  24 |     await page.goto("/fr/publier");
  25 |     const title = `Test E2E ${Date.now()}`;
> 26 |     await page.getByLabel(/titre/i).fill(title);
     |                                     ^ Error: locator.fill: Test timeout of 60000ms exceeded.
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