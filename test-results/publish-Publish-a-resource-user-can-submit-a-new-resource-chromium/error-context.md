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
          - generic [ref=e21]: fr
        - button "Aide" [ref=e22]:
          - link [ref=e23] [cursor=pointer]:
            - /url: /aide
            - img [ref=e24]
        - button "Mon compte" [ref=e28]: C
  - generic [ref=e29]:
    - generic [ref=e30]:
      - link "Quitter le mode rédaction" [ref=e31] [cursor=pointer]:
        - /url: /catalogue
        - img [ref=e32]
        - text: Quitter le mode rédaction
      - generic [ref=e35]: Brouillon...
    - generic [ref=e36]:
      - generic:
        - heading "Ressource sans titre" [level=1]
        - paragraph: Commencez à partager vos connaissances. Le monde vous écoute.
      - textbox "Donnez un titre à votre ressource..." [ref=e38]
      - generic [ref=e40]:
        - generic [ref=e41]:
          - button "H1" [ref=e42]
          - button "H2" [ref=e43]
          - button "H3" [ref=e44]
          - button "G" [ref=e46]:
            - strong [ref=e47]: G
          - button "I" [ref=e48]:
            - emphasis [ref=e49]: I
          - button "S" [ref=e50]
          - button "<>" [ref=e51]
          - button "≡" [ref=e53]
          - button "1≡" [ref=e54]
          - button "❝" [ref=e56]
          - 'button "{ }" [ref=e57]'
          - button "―" [ref=e58]
          - button "↩" [ref=e60]
          - button "↪" [ref=e61]
        - paragraph [ref=e64]: Commencez à rédiger votre ressource ici...
      - separator [ref=e65]
      - generic [ref=e66]:
        - generic [ref=e67]:
          - generic [ref=e68]: Catégorie
          - combobox [ref=e69] [cursor=pointer]:
            - option "Choisir un chemin..." [selected]
            - option "Anxiété & Stress"
            - option "Équilibre vie pro/perso"
            - option "Parentalité"
            - option "Soutien de crise"
            - option "Santé mentale"
        - generic [ref=e70]:
          - generic [ref=e71]: Visibilité
          - generic [ref=e72]:
            - generic [ref=e73] [cursor=pointer]:
              - radio "Public— Visible par toute la communauté" [checked] [ref=e75]
              - generic [ref=e78]:
                - text: Public
                - generic [ref=e79]: — Visible par toute la communauté
            - generic [ref=e80] [cursor=pointer]:
              - radio "Privé— Accessible uniquement par vous" [ref=e82]
              - generic [ref=e84]:
                - text: Privé
                - generic [ref=e85]: — Accessible uniquement par vous
      - generic [ref=e86]:
        - generic [ref=e87]: Format
        - combobox [ref=e88] [cursor=pointer]:
          - option "Article / Réflexion" [selected]
          - option "Vidéo"
          - option "Document PDF"
          - option "Exercice"
          - option "Audio / Podcast"
          - option "Protocole"
      - generic [ref=e89]:
        - generic [ref=e90]: Image de couverture
        - button "Ajouter une image de couverture JPG, PNG, WebP — recommandé 1200×400 px" [ref=e91]:
          - img [ref=e92]
          - generic [ref=e96]: Ajouter une image de couverture
          - generic [ref=e97]: JPG, PNG, WebP — recommandé 1200×400 px
        - button "Choose File" [ref=e98]
      - generic [ref=e99]:
        - img [ref=e100]
        - paragraph [ref=e102]: Joignez des documents, vidéos ou fichiers audio
        - button "Parcourir les fichiers" [ref=e103]
        - button "Choose File" [ref=e104]
      - generic [ref=e105]:
        - button "Publier sur (RE)Sources" [ref=e106]
        - button "Enregistrer comme brouillon" [ref=e107]
    - contentinfo [ref=e108]:
      - generic [ref=e109]:
        - paragraph [ref=e110]: © 2024 (RE)Sources Relationnelles. Une initiative officielle de santé publique.
        - generic [ref=e111]:
          - button "Confidentialité" [ref=e112]
          - button "Accessibilité" [ref=e113]
          - button "Assistance" [ref=e114]
  - contentinfo [ref=e115]:
    - generic [ref=e116]:
      - paragraph [ref=e117]: © 2024 (RE)Sources Relationnelles. Initiative officielle de santé publique.
      - generic [ref=e118]:
        - link "Mentions légales" [ref=e119] [cursor=pointer]:
          - /url: /mentions-legales
        - link "Accessibilité" [ref=e120] [cursor=pointer]:
          - /url: /accessibilite
        - link "Confidentialité" [ref=e121] [cursor=pointer]:
          - /url: /confidentialite
        - link "Contact Support" [ref=e122] [cursor=pointer]:
          - /url: /contact
  - button "Open Next.js Dev Tools" [ref=e128] [cursor=pointer]:
    - img [ref=e129]
  - alert [ref=e132]
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