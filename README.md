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

---
*Conçu pour La Puce Ressource Informatique*
