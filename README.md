# Prompt-Bloc-Notion - La Puce

**Découvrez des idées de prompts** est un outil de génération de prompts IA simple et épuré, pensé spécifiquement pour le contexte communautaire et les organismes à but non lucratif (OBNL). Il est optimisé pour être hébergé sur GitHub Pages et intégré ('embed') au sein de bases de connaissances comme Notion.

## Fonctionnalités ⚙️
*   **Génération ciblée** : Crée des prompts précis basés sur des tâches pré-définies ou des saisies personnalisées.
*   **Personnalisation UX** : Paramétrage du ton (Amical, Soutien, Analytique, Simple) et du niveau de détail (Direct, Standard, Approfondi).
*   **Historique Local** : Sauvegarde des prompts générés dans le `localStorage` de l'utilisateur. Export possible au format texte.
*   **Embed-Friendly** : Optimisé pour un affichage "responsive" parfait dans un iframe Notion ou sur mobile.
*   **Sécurisé** : 
    * Appels API masqués derrière un Cloudflare Worker Proxy.
    * Protection front-end stricte contre le Cross-Site Scripting (XSS) via `DOMPurify`.
    * Content-Security-Policy (CSP) durcie.

## Architecture & Fichiers

L'application est découpée en deux services pour des raisons de sécurité absolue :
1.  **Frontend Statique (GitHub Pages)**
    *   `index.html` : L'interface principale.
    *   `app.js` : La gestion des événements (DOM, clics, appels API proxy).
    *   `lang.js` : Dictionnaire bilingue FR/EN.
    *   `style.min.css` : Styles compilés (utilisant Tailwind CSS).
2.  **API Proxy (Cloudflare Workers)**
    *   Fichier(s) `worker*.js` : Reçoivent l'appel du frontend, appliquent des limites, consultent secrètement l'API IA (ex: Gemini ou HuggingFace) et retournent la réponse structurée au frontend. 

## Guide de Déploiement

### 1. Héberger le Frontend (GitHub Pages)
Seuls les fichiers de distribution sont nécessaires. (Voir le fichier `.gitignore` pour la liste des éléments exclus, comme les scripts Node.js ou `input.css`).
1. Créez un dépôt GitHub vide.
2. Poussez les fichiers suivants sur une branche (ex: `main`) :
    *   `index.html`
    *   `app.js`
    *   `lang.js`
    *   `style.min.css`
    *   Dossier `assets/` (avec `logo.png`)
3. Dans les paramètres GitHub du repo (Settings > Pages), activez GitHub Pages sur la branche `main`. Votre outil sera en ligne.

### 2. Héberger le Proxy (Cloudflare)
Le fichier `worker.js` (ou les versions spécifiques à une API) **ne doit pas** être poussé sur une branche publique.
1. Connectez-vous à Cloudflare Workers.
2. Créez un nouveau Worker.
3. Copiez/collez le contenu de `worker.js` dans l'éditeur Cloudflare.
4. Renseignez l'URL de votre site sur la ligne `ALLOWED_ORIGIN` (ex: `"https://votre-compte.github.io"`).
5. Sauvegardez et déployez. L'URL générée par Cloudflare (finissant par `.workers.dev`) est celle que `app.js` doit utiliser.

## Développement technique

*   **CSS** : Utilisation de Tailwind CSS 3 via CDN interne. Les commandes de compilation nécessitent Node.js : `npx tailwindcss -i ./input.css -o ./style.min.css --minify`
*   **Sécurité** : L'injection de contenu dynamique utilise exclusivement `DOMPurify.sanitize()` avec une restriction sur les attributs HTML pour sécuriser `innerHTML`.

---
*Conçu pour La Puce Ressource Informatique*
