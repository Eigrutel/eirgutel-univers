# Architecture — Univers

## Vue d’ensemble

Univers est une **application web autonome contenue dans un unique fichier `univers.html`**.

Aucun framework, serveur ou système de compilation n’est nécessaire pour l’exécuter. Le HTML contient directement :

- la structure de l’interface ;
- les styles CSS ;
- les données du modèle par défaut français et anglais ;
- la logique JavaScript.

Cette architecture permet de télécharger le fichier et de l’utiliser localement dans un navigateur.

## Organisation générale

```text
univers.html
├── en-tête et métadonnées
├── styles CSS
├── structure HTML de l’interface
└── JavaScript
    ├── constantes et traductions
    ├── modèle par défaut FR / EN
    ├── état de l’application
    ├── cartes, fiches et liens
    ├── relations intercartes
    ├── stockage et historique
    ├── rendu de l’interface
    ├── recherche
    ├── interactions et gestes
    ├── zoom et navigation
    ├── exports PDF / JSON
    ├── diaporamas
    └── événements
```

## État principal

L’état courant est centralisé dans l’objet `S`.

Il contient notamment :

- le titre du projet ;
- la carte actuellement ouverte ;
- le niveau de zoom ;
- les réglages d’interface ;
- toutes les cartes ;
- les relations intercartes ;
- la sélection active ;
- l’état des interactions ;
- la recherche ;
- le diaporama.

## Cartes

Chaque carte est un objet disposant notamment de :

- `id` : identifiant stable ;
- `code` : code hiérarchique ;
- `title` : titre ;
- `parentMapId` : carte parente éventuelle ;
- `parentNodeId` : fiche parente éventuelle ;
- `start` : fiche centrale ;
- `nodes` : fiches de la carte ;
- `links` : relations internes.

La carte `overview` constitue la vue générale.

## Fiches

Une fiche contient notamment :

- un identifiant ;
- un code hiérarchique ;
- un titre ;
- un texte ;
- une position X/Y ;
- une couleur ;
- une image éventuelle ;
- un état replié / déplié ;
- une éventuelle mindmap liée.

## Liens internes

Les liens d’une carte sont conservés dans `map.links`.

Deux grandes familles sont utilisées :

- liens hiérarchiques de type `suite` ;
- relations libres de type `relation`.

Les liens hiérarchiques servent également à calculer les descendants et l’organisation des branches.

## Relations intercartes

Les relations entre des fiches appartenant à des cartes différentes sont stockées séparément dans `S.crossLinks`.

Chaque relation conserve les identifiants :

- de la carte source ;
- de la fiche source ;
- de la carte cible ;
- de la fiche cible ;

ainsi qu’un libellé et une note facultatifs.

## Modèle par défaut

Les modèles sont intégrés directement dans le JavaScript :

- `CATEGORY_DATA_FR`
- `CATEGORY_DATA_EN`

Ils servent uniquement à créer l’univers initial. Une fois créé, le contenu appartient aux données de l’utilisateur et peut être entièrement modifié.

## Langues

Les textes d’interface et les modèles français / anglais sont intégrés au fichier.

Le réglage de langue est conservé dans les paramètres de l’état et dans les données exportées.

## Stockage local

L’application utilise `localStorage` pour conserver automatiquement l’univers courant.

La clé historique de stockage reste :

```text
stripmee_univers_v15
```

Elle est volontairement conservée afin de ne pas rendre inaccessibles des données locales existantes lors du passage à la version publique 1.0.0.

Des sauvegardes secondaires datées sont également maintenues dans le navigateur.

## Historique

L’annulation et le rétablissement reposent sur des instantanés sérialisés de l’état.

Deux piles sont maintenues :

- `undo`
- `redo`

Le nombre d’états conservés est limité afin d’éviter une croissance indéfinie de la mémoire.

## Import / export JSON

L’export produit un objet contenant notamment :

- l’identifiant de l’application ;
- la version ;
- la langue ;
- la date de mise à jour ;
- le titre du projet ;
- la carte courante ;
- la vue ;
- les réglages ;
- les cartes ;
- les relations intercartes.

Le JSON constitue le format principal de sauvegarde et de transfert d’un univers.

## Images

Deux méthodes sont possibles :

- fichier local converti et intégré aux données ;
- URL externe.

Une image intégrée augmente la taille du JSON mais permet de conserver un ensemble autonome.

## Exports documentaires

Univers propose plusieurs sorties :

- PDF d’un élément ;
- PDF d’une carte ;
- Livre-monde ;
- diaporama interactif ;
- diaporama linéaire ;
- export HTML autonome des diaporamas.

## Affichage et interactions

Le plateau utilise un grand espace de travail à coordonnées absolues.

Le zoom est appliqué par transformation CSS. Les positions des fiches restent exprimées dans le repère logique du plateau.

Sur mobile, l’interface passe en mode colonne unique et adapte les gestes, l’éditeur et les commandes.

## Dépendances

Aucune bibliothèque JavaScript externe n’est requise pour le fonctionnement principal.

Univers est conçu pour rester exploitable comme fichier HTML autonome.

## Fichiers du dépôt

- `univers.html` — application ;
- `index.html` — page de présentation GitHub Pages ;
- `README.md` — présentation et utilisation ;
- `NOTICE.md` — auteur, licences et marques ;
- `LICENSE.md` — répartition des licences ;
- `CHANGELOG.md` — historique des versions ;
- `ARCHITECTURE.md` — présent document ;
- `docs/` — documentation imprimable du modèle par défaut.
