# Cosmos Explorer — Système Solaire 3D

Un orrery 3D interactif et en temps réel du Système Solaire, construit dans un seul fichier `index.html` autonome. Il affiche le Soleil, les huit planètes, les anneaux de Saturne, la Lune terrestre, une ceinture d'astéroïdes, la région de Kuiper, des comètes et des étoiles filantes, avec une interface tête haute vitrée pour explorer chaque corps céleste.

## Fonctionnalités

- **Scène 3D en temps réel** propulsée par [Three.js](https://threejs.org/) avec post-processing bloom, un champ d'étoiles profond, une couche de nébuleuse et des halos atmosphériques.
- **Huit planètes** avec des surfaces texturées, une inclinaison axiale, un mouvement orbital, les anneaux de Saturne et les nuages + la Lune de la Terre.
- **Cliquer pour explorer** : sélectionnez n'importe quel corps pour y faire voler la caméra et ouvrir le panneau d'inspection (diamètre, distance, période orbitale, température de surface, lunes, description, tags).
- **Modes caméra** : Orbital (glisser/zoomer), mode Pilote/vol (ZQSD + souris) et une visite Cinématique automatique.
- **Interrupteurs de scène** : lignes d'orbite, étiquettes, ceinture d'astéroïdes, météores et nébuleuse.
- **Orbites réalistes (optionnel)** : un interrupteur dans les réglages passe de la vue circulaire plate à de véritables orbites elliptiques inclinées (excentricité, inclinaison et nœud par planète), avec un **curseur d'échelle des distances** pour comprimer ou étendre le système. La Lune possède désormais sa propre étiquette.
- **Contrôle de la qualité** : Auto / Élevée / Moyenne / Basse. *Auto* surveille le taux d'images en temps réel et ajuste la qualité à la hausse ou à la baisse (ratio de pixels, bloom, étoiles proches) pour maintenir la fluidité. Un indicateur FPS en direct se trouve dans le panneau de réglages.
- **Données en temps réel** :
  - **Positions réelles** — un interrupteur dans les réglages place les planètes à leurs positions héliocentriques *réelles* pour la date du jour, calculées localement à partir des éléments orbitaux approximatifs du JPL (pas d'API, fonctionne hors ligne). L'horloge de simulation affiche alors la date calendaire réelle et le curseur d'accélération temporelle avance dans l'éphéméride réel.
  - **Météo spatiale en direct** — récupère l'**indice Kp** planétaire actuel et la **vitesse du vent solaire** depuis les flux JSON publics en temps réel du NOAA SWPC, affichés dans le HUD et dans l'inspecteur du Soleil. C'est la seule fonctionnalité qui effectue une requête réseau ; elle échoue silencieusement et se dégrade gracieusement en mode hors ligne ou bloqué.
- **Contrôles temporels** : lecture/pause et un curseur d'accélération temporelle (0,1× à 50×) avec une horloge de simulation en direct.
- **Radar local** et affichage des coordonnées galactiques.
- **Interface bilingue (FR / EN)** avec un bouton de changement de langue en un clic.
- **Responsive / adapté au mobile** : sur téléphone, l'inspecteur devient un panneau glissant depuis le bas, les boutons de langue et de réglages restent visibles, et le moteur de rendu réduit automatiquement sa charge (moins d'étoiles et d'astéroïdes, ratio de pixels et bloom réduits) pour des performances plus fluides. Les gestes tactiles permettent de pivoter et de zoomer par pincement, et le **mode pilote dispose d'un joystick à l'écran** (déplacement), glisser pour regarder, et boutons haut/bas.

## Changement de langue (FR / EN)

L'interface est entièrement bilingue. Un bouton **FR / EN** se trouve dans le groupe de contrôles en haut à droite (à côté de *Cinématique*).

- Le bouton affiche la langue vers laquelle vous pouvez basculer (`FR` en anglais, `EN` en français).
- Cliquer dessus traduit l'intégralité de l'interface : télémétrie du HUD, panneaux latéraux, inspecteur, console inférieure, interrupteurs de scène, overlay pilote, indices, et tous les noms, descriptions et tags des planètes (y compris les étiquettes 3D).
- Votre choix est sauvegardé dans le navigateur (clé `localStorage` `cosmosLang`) et restauré automatiquement à la prochaine visite.
- Le changement de langue rafraîchit les panneaux sans déplacer la caméra, votre vue actuelle est donc préservée.

La langue par défaut est l'anglais. Pour changer la langue par défaut, définissez `localStorage.cosmosLang` à `"fr"` ou `"en"`.

## Démarrage rapide

Aucune étape de build ni de dépendances à installer — tout est chargé depuis des CDN (Three.js, Tailwind, Google Fonts) via un import map.

1. Clonez ou téléchargez ce dépôt.
2. Ouvrez `index.html` dans un navigateur moderne.

Pour que les textures et les modules CDN se chargent correctement, il est préférable de servir le fichier via un serveur web local plutôt que de l'ouvrir directement avec `file://` :

```bash
# Python 3
python -m http.server 8000
```

Puis visitez `http://localhost:8000`.

## Contrôles

| Action | Contrôle |
| --- | --- |
| Orbiter la caméra | Glisser (Maj+glisser) |
| Zoomer | Molette / pincer |
| Sélectionner un corps | Cliquer sur une planète ou un élément de la liste |
| Pause / reprendre | Espace |
| Mode vol (pilote) | `F` — puis ZQSD déplacer, A/E roulis, R/F haut/bas, souris pour regarder |
| Mode pilote (mobile) | Réglages → Pilote — joystick pour bouger, glisser pour regarder, ▲/▼ pour l'altitude |
| Quitter pilote / cinématique | `Échap` (ou le bouton Quitter sur mobile) |
| Ouvrir les réglages (qualité, échelle, orbites) | bouton ⚙ |
| Changer de langue | bouton FR / EN |
| Ouvrir les infos planète (mobile) | Taper sur une planète, ou le bouton ℹ |
| Fermer la fiche info (mobile) | bouton ✕ sur la fiche |

## Stack technique

- [Three.js](https://threejs.org/) (`OrbitControls`, `EffectComposer`, `UnrealBloomPass`)
- [Tailwind CSS](https://tailwindcss.com/) (build navigateur)
- JavaScript vanilla (modules ES), architecture mono-fichier

## Sources de données

- Textures des planètes : Solar System Scope via Wikimedia Commons.
- Éphéméride en direct : JPL « Approximate Positions of the Planets » (éléments képlériens, valide ~1800–2050).
- Météo spatiale en direct : [NOAA SWPC](https://services.swpc.noaa.gov/) produits JSON en temps réel (indice K planétaire, plasma du vent solaire). Domaine public.

## Licence

Ce projet est open source. N'hésitez pas à l'utiliser, le modifier et le partager.
