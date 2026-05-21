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
        - link "Catalogue" [ref=e6] [cursor=pointer]:
          - /url: /catalogue
        - link "Bien-être" [ref=e7] [cursor=pointer]:
          - /url: /bien-etre
        - link "Communauté" [ref=e8] [cursor=pointer]:
          - /url: /communaute
        - link "Urgence" [ref=e9] [cursor=pointer]:
          - /url: /urgence
      - generic [ref=e10]:
        - button "Rechercher" [ref=e11]:
          - img [ref=e12]
          - generic [ref=e15]: Rechercher...
        - button "Langue" [ref=e17]:
          - img [ref=e18]
          - generic [ref=e22]: fr
        - button "Aide" [ref=e23]:
          - link [ref=e24] [cursor=pointer]:
            - /url: /aide
            - img [ref=e25]
        - button "Mon compte" [ref=e30]: C
  - generic [ref=e31]:
    - generic [ref=e32]:
      - link "Quitter le mode rédaction" [ref=e33] [cursor=pointer]:
        - /url: /catalogue
        - img [ref=e34]
        - text: Quitter le mode rédaction
      - generic [ref=e37]: Brouillon...
    - generic [ref=e38]:
      - generic:
        - heading "Ressource sans titre" [level=1]
        - paragraph: Commencez à partager vos connaissances. Le monde vous écoute.
      - textbox "Donnez un titre à votre ressource..." [ref=e40]
      - generic [ref=e42]:
        - generic [ref=e43]:
          - button "H1" [ref=e44]
          - button "H2" [ref=e45]
          - button "H3" [ref=e46]
          - button "G" [ref=e48]:
            - strong [ref=e49]: G
          - button "I" [ref=e50]:
            - emphasis [ref=e51]: I
          - button "S" [ref=e52]
          - button "<>" [ref=e53]
          - button "≡" [ref=e55]
          - button "1≡" [ref=e56]
          - button "❝" [ref=e58]
          - 'button "{ }" [ref=e59]'
          - button "―" [ref=e60]
          - button "↩" [ref=e62]
          - button "↪" [ref=e63]
        - paragraph [ref=e66]: Commencez à rédiger votre ressource ici...
      - separator [ref=e67]
      - generic [ref=e68]:
        - generic [ref=e69]:
          - generic [ref=e70]: Catégorie
          - combobox [ref=e71] [cursor=pointer]:
            - option "Choisir un chemin..." [selected]
            - option "Anxiété & Stress"
            - option "Équilibre vie pro/perso"
            - option "Parentalité"
            - option "Soutien de crise"
            - option "Santé mentale"
        - generic [ref=e72]:
          - generic [ref=e73]: Visibilité
          - generic [ref=e74]:
            - generic [ref=e75] [cursor=pointer]:
              - radio "Public— Visible par toute la communauté" [checked] [ref=e77]
              - generic [ref=e80]:
                - text: Public
                - generic [ref=e81]: — Visible par toute la communauté
            - generic [ref=e82] [cursor=pointer]:
              - radio "Privé— Accessible uniquement par vous" [ref=e84]
              - generic [ref=e86]:
                - text: Privé
                - generic [ref=e87]: — Accessible uniquement par vous
      - generic [ref=e88]:
        - generic [ref=e89]: Format
        - combobox [ref=e90] [cursor=pointer]:
          - option "Article / Réflexion" [selected]
          - option "Vidéo"
          - option "Document PDF"
          - option "Exercice"
          - option "Audio / Podcast"
          - option "Protocole"
      - generic [ref=e91]:
        - generic [ref=e92]: Image de couverture
        - button "Ajouter une image de couverture JPG, PNG, WebP — recommandé 1200×400 px" [ref=e93]:
          - img [ref=e94]
          - generic [ref=e100]: Ajouter une image de couverture
          - generic [ref=e101]: JPG, PNG, WebP — recommandé 1200×400 px
        - button "Choose File" [ref=e102]
      - generic [ref=e103]:
        - img [ref=e104]
        - paragraph [ref=e106]: Joignez des documents, vidéos ou fichiers audio
        - button "Parcourir les fichiers" [ref=e107]
        - button "Choose File" [ref=e108]
      - generic [ref=e109]:
        - button "Publier sur (RE)Sources" [ref=e110]
        - button "Enregistrer comme brouillon" [ref=e111]
    - contentinfo [ref=e112]:
      - generic [ref=e113]:
        - paragraph [ref=e114]: © 2024 (RE)Sources Relationnelles. Une initiative officielle de santé publique.
        - generic [ref=e115]:
          - button "Confidentialité" [ref=e116]
          - button "Accessibilité" [ref=e117]
          - button "Assistance" [ref=e118]
  - contentinfo [ref=e119]:
    - generic [ref=e120]:
      - paragraph [ref=e121]: © 2024 (RE)Sources Relationnelles. Initiative officielle de santé publique.
      - generic [ref=e122]:
        - link "Mentions légales" [ref=e123] [cursor=pointer]:
          - /url: /mentions-legales
        - link "Accessibilité" [ref=e124] [cursor=pointer]:
          - /url: /accessibilite
        - link "Confidentialité" [ref=e125] [cursor=pointer]:
          - /url: /confidentialite
        - link "Contact Support" [ref=e126] [cursor=pointer]:
          - /url: /contact
  - button "Open Next.js Dev Tools" [ref=e132] [cursor=pointer]:
    - img [ref=e133]
  - alert [ref=e137]
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