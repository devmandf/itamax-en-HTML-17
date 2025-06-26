# Guide de Contribution

Merci de prendre un moment pour lire ce guide avant de faire des modifications. Le but est de maintenir la cohérence et la stabilité du code.

## Architecture Générale

1.  **JavaScript Vanilla :** Le projet est écrit en JavaScript pur (Vanilla JS), sans jQuery ou autre bibliothèque. C'est un choix de simplicité et de performance. Toute nouvelle fonctionnalité doit respecter ce principe.

2.  **CSS Centralisé :** Tous les styles sont dans un unique fichier, `styles.css`. Merci de ne pas ajouter de balises `<style>` directement dans les fichiers HTML.

3.  **Site Statique :** Le site est composé de pages HTML statiques. Les éléments communs comme le header et le footer sont dupliqués manuellement.

## Conventions de Code et Formats

-   **Langue :** Tout le contenu visible, ainsi que les commentaires de code, doivent être en **français**.
-   **Organisation CSS :** Pour la clarté, les différentes sections du fichier `styles.css` sont organisées avec des bannières de commentaires (ex: `/* ===== STYLES POUR LA SECTION PROJET ===== */`).
-   **Formats d'Images :** Pour optimiser le temps de chargement, les images de contenu des projets doivent utiliser le format **`.webp`**. Les icônes et logos doivent être au format **`.svg`**.

## La Galerie de Projets : La Règle d'Or

C'est la partie la plus complexe du site et elle suit une règle très stricte. **Ne pas la respecter cassera l'affichage des titres.**

-   **Structure :** Chaque page contenant une galerie (`amo.html`, `appins.html`, `batins.html`, etc.) doit contenir une section `<section class="project-section">`.
-   **Données Locales :** Les informations de la galerie (titres, images) sont définies dans une variable `const projects` à la fin de chaque fichier HTML concerné. C'est ce qui permet à chaque galerie d'avoir son propre contenu.
-   **Logique Globale :** Le fichier `script.js` contient **toute** la logique d'affichage. Il est conçu pour s'adapter et fonctionner uniquement si les éléments HTML nécessaires (comme `.project-section`) sont présents sur la page, ce qui lui permet d'être inclus partout sans causer d'erreur. **Il ne faut JAMAIS ajouter de code de gestion de galerie directement dans un fichier HTML.**

### Le "Puzzle du Millénaire" : Pourquoi le script des titres est si complexe

Le script dans `script.js` qui gère l'affichage des titres (`adjustAndSplitText`) a été spécifiquement conçu pour résoudre plusieurs problèmes complexes :
- Il force les titres et sous-titres sur deux lignes pour une mise en page stable.
- Il ajuste la taille de la police pour que le texte ne soit jamais coupé.
- Il utilise `requestAnimationFrame` pour éviter les bugs de synchronisation avec le rendu du navigateur.
- Il gère dynamiquement l'affichage/masquage des éléments de navigation.

**Ne modifiez pas cette fonction à la légère. Elle est le fruit de nombreuses itérations et résout des problèmes qui ne sont pas visibles à première vue.**

### Bonnes Pratiques pour les Modifications de Style

1. **Navigation Galerie** :
   - Les flèches de navigation supérieures sont masquées par défaut (`.top-nav .nav-arrow { display: none; }`)
   - La navigation par swipe est limitée à la zone de l'image pour éviter les déclenchements intempestifs
   - Le zoom est désormais possible sans déclencher de changement d'image
   - Pour toute modification de style, s'assurer de ne pas affecter la navigation principale

2. **Responsive Design** :
   - Toujours tester les modifications sur différentes tailles d'écran
   - Utiliser les media queries existantes pour les ajustements spécifiques
   - Vérifier que la navigation reste fonctionnelle sur mobile

3. **Accessibilité** :
   - La navigation doit rester accessible au clavier
   - Les contrôles doivent avoir des états visuels distincts (hover, focus, active)

### Navigation dans la Galerie d'Images

La navigation dans la galerie d'images suit des règles strictes pour assurer une expérience utilisateur cohérente :

#### Boutons de Navigation
- **Flèches inférieures uniquement** : Navigation entre les projets et les images
  - Flèche gauche : Image précédente (ou dernier projet si sur la première image)
  - Flèche droite : Image suivante (ou projet suivant si sur la dernière image)
  
> **Note :** Les flèches supérieures ont été masquées pour simplifier l'interface. La navigation complète est disponible via les flèches inférieures.

#### Raccourcis Clavier
- **Flèche gauche** : Même comportement que le bouton "Image précédente"
  - Si première image : passe au dernier projet
  - Sinon : image précédente

- **Flèche droite** : Même comportement que le bouton "Image suivante"
  - Si dernière image : passe au projet suivant
  - Sinon : image suivante

- **Maj + Flèche gauche** : Projet précédent
- **Maj + Flèche droite** : Projet suivant

#### Comportement Spécial pour les Projets à Une Seule Image
- Les flèches de navigation d'images restent actives
- Le clic sur ces flèches passe directement au projet précédent/suivant
- Ce comportement permet une navigation fluide même pour les projets avec une seule image

#### Points Importants
1. Les boutons de navigation sont toujours visibles mais peuvent être désactivés lorsqu'aucune action n'est possible
2. La navigation est circulaire : après le dernier projet, on revient au premier
3. Les logs de débogage sont activés pour faciliter la résolution des problèmes

**Attention** : Toute modification de ces comportements doit être testée sur des projets avec un nombre variable d'images (0, 1, plusieurs) pour s'assurer de la stabilité du système.

### Amélioration de la Navigation sur Mobile (Juin 2024)

1. **Navigation par Swipe Optimisée**
   - La détection du swipe est maintenant limitée à la zone de l'image
   - Meilleure distinction entre les gestes de zoom et de navigation
   - Seuls les swipes rapides et nets déclenchent le changement d'image
   - Prévention des conflits avec le zoom tactile

2. **Amélioration de l'Expérience Utilisateur**
   - Les gestes à deux doigts sont désormais correctement interprétés comme un zoom
   - Réduction des faux positifs lors de la navigation
   - Meilleure réactivité sur les appareils tactiles

### Historique des Interventions (Jules - Été 2024)

Plusieurs ajustements et corrections ont été apportés pour améliorer l'expérience utilisateur et la robustesse du site :

1.  **Correction du décalage du contenu par le menu déroulant (Tablette) :**
    *   **Problème :** Sur les écrans de taille tablette (entre 577px et 992px), l'ouverture du menu déroulant principal provoquait un décalage vers le haut du contenu situé en dessous de l'en-tête.
    *   **Solution :** L'en-tête (`.header`) a été rendu `position: fixed;` dans cette plage de résolutions, alignant son comportement sur celui des vues mobile et bureau. Cela empêche le menu d'affecter le flux du contenu principal.
    *   *Fichiers modifiés : `styles.css`*
    *   *Branche : `fix/tablet-dropdown-shift`*

2.  **Correction des boutons de navigation principaux masqués par l'en-tête :**
    *   **Problème :** Certains boutons de navigation principaux (ex: "AMO", "Appuis institutionnels") étaient masqués derrière l'en-tête sur des largeurs d'écran inférieures à 992px à cause d'une marge supérieure négative (`margin-top: -45px;`).
    *   **Solution :** La marge supérieure négative du `.button-container` a été réinitialisée à `0` pour les écrans `<= 992px`, permettant aux boutons de s'afficher correctement en dessous de l'en-tête fixe.
    *   *Fichiers modifiés : `styles.css`*
    *   *Branche : `fix/tablet-dropdown-shift`*

3.  **Optimisation complète de la page de contact pour tous les appareils :**
    *   **Améliorations apportées :**
        * Mise en place d'un système de media queries avancé pour les écrans de 769px à 992px
        * Ajustements précis des marges et espacements pour chaque plage de résolution
        * Optimisation spécifique pour les très petits écrans (< 348px)
        * Amélioration de la lisibilité sur toutes les tailles d'écran
    *   **Points de rupture principaux :**
        * 769px - 809px : Ajustement des marges pour les tablettes
        * 809px - 913px : Optimisation pour les tablettes en mode paysage
        * 914px - 924px : Ajustements fins pour les écrans intermédiaires
        * 925px - 992px : Optimisation pour les petits ordinateurs portables
        * < 348px : Mode très petit écran avec réduction proportionnelle
    *   **Optimisations spécifiques :**
        * Taille de police réactive basée sur la largeur de la fenêtre
        * Ajustement dynamique des marges et espacements
        * Amélioration de l'alignement des éléments sur tous les appareils
        * Optimisation des boutons et éléments interactifs pour le toucher
    *   *Fichiers modifiés : `contact.html` (styles embarqués)*
    *   *Branche : `fix/contact-page-spacing`*

4.  **Stabilisation des flèches "Suivant" de la galerie de projets :**
    *   **Problème :** Les flèches "Suivant" (droite) des galeries de projets (en haut et en bas de l'image) présentaient un léger mouvement horizontal lors de l'interaction (clic/focus).
    *   **Solution :**
        *   Une première tentative avec `outline: none;` n'a pas suffi.
        *   La cause identifiée était une potentielle instabilité dimensionnelle du bouton fléché, exacerbée par sa structure HTML imbriquée (contrairement aux flèches "Précédent") et les propriétés flex de son parent.
        *   La solution finale a consisté à attribuer une `width` et `height` explicites aux boutons `.nav-arrow` (correspondant à la taille de l'image qu'ils contiennent) et à ajouter `box-sizing: border-box;`. Ceci assure une dimension fixe au bouton, empêchant les micro-variations de rendu de l'SVG ou de la boîte de causer un recalcul de layout.
    *   *Fichiers modifiés : `styles.css`*
    *   *Branche : `feat/contact-updates-and-arrow-fix`*

5.  **Ajout des numéros de téléphone sur la page de contact :**
    *   **Demande :** Intégrer deux numéros de téléphone sous l'adresse e-mail sur `contact.html`.
    *   **Solution :** Deux nouveaux paragraphes contenant les numéros ont été ajoutés. Ils ont été stylisés (couleur orange, police Jura, taille adaptée pour mobile et bureau) de manière cohérente avec l'adresse e-mail, en utilisant des styles CSS embarqués et en tenant compte de la grille flexible parente pour l'espacement.
    *   *Fichiers modifiés : `contact.html` (HTML et styles embarqués)*
    *   *Branche : `feat/contact-updates-and-arrow-fix`*
