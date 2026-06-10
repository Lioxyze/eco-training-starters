# Backlog — Éco-conception Vinted (repo miroir : heavy-shop)

## Contexte du projet

- **Service réel analysé :** Vinted.fr (marketplace C2C de seconde main entre particuliers).
- **Catégorie :** Marketplace C2C / e-commerce / catalogue d'annonces.
- **Repo miroir :** `heavy-shop` (boutique catalogue : accueil, listing, fiche produit, recherche/filtres, panier, checkout).
- **Unité fonctionnelle (UF) :** « Publier une annonce et la rendre consultable au catalogue » — c'est-à-dire le **cycle de vie d'une annonce mise en ligne** : ses médias, son contenu servi sur la fiche, et sa fin de vie.

---

## User story 1 — Optimiser et compresser les images de l'annonce

- **Contexte :** En tant que vendeur publiant une annonce, je veux que les photos de mon article soient servies dans un poids et un format adaptés, afin que l'annonce mise en ligne ne transfère pas de médias surdimensionnés à chaque consultation.
- **Objectif :** Convertir au moins **75 % des images** d'annonce vers un format performant (WebP/AVIF), appliquer une compression cible (~72 % WebP / ~56 % AVIF), et ne charger d'emblée que **l'image principale** en différant les **4 miniatures** (lazy-load) → réduire d'au moins **40 % le poids des médias** de la fiche produit.
- **Bonne pratique d'éco-conception ciblée :** Utiliser un format d'image performant (SVG, WebP, AVIF) et un niveau de compression adapté au contenu et au contexte de visualisation (RGESN 2024 — thème Contenus, critères 5.1 « format de fichier adapté pour chaque image » et 5.2 « niveau de compression des images adapté »).
- **KPI associé :** Poids total des images chargées sur la fiche produit (Ko) + nombre de requêtes image (DevTools → onglet Réseau), et score EcoIndex de la page fiche produit.
- **Repo / écran concerné :** Fiche produit / galerie d'images de l'annonce (image principale + miniatures) / page catalogue.
- **Critère de réussite :** Poids des médias de la fiche produit réduit d'au moins 40 %, miniatures chargées à la demande (lazy-load), sans perte de qualité perçue.
- **Niveau de priorité :** Haute.
- **Planification (roadmap 6 mois) :** M2 (optimisation des images) → M4 (lazy-load des miniatures).

---

## User story 2 — Alléger le payload de l'annonce publiée

- **Contexte :** En tant que plateforme, je veux que la donnée renvoyée pour une annonce publiée soit limitée à l'utile, afin de réduire le volume transféré et le calcul serveur à chaque consultation de fiche.
- **Objectif :** Supprimer **100 % des champs dupliqués ou non affichés** de la réponse produit (`longDescription` dupliquée, `duplicateMarketingCopy`, `stockHistory` injecté) et ne renvoyer que les données réellement utilisées → réduire d'au moins **30 % le poids de la réponse JSON** de `GET /api/products/:id`.
- **Bonne pratique d'éco-conception ciblée :** Réduire la volumétrie de données échangées avec le serveur en évitant les transferts et requêtes inutiles — payload superflu, champs dupliqués, données injectées (RGESN 2024 — thème UX/UI, critère 4.9 « limiter les requêtes serveur / réduire la volumétrie de données échangées »).
- **KPI associé :** Poids de la réponse JSON de `GET /api/products/:id` (Ko) + nombre de champs renvoyés non affichés à l'écran.
- **Repo / écran concerné :** Fiche produit / réponse de l'API produit / page catalogue.
- **Critère de réussite :** Payload de la fiche produit réduit d'au moins 30 %, suppression des champs dupliqués / non utilisés, sans régression fonctionnelle.
- **Niveau de priorité :** Moyenne.
- **Planification (roadmap 6 mois) :** M2 (allègement du payload produit).

---

## User story 3 — Supprimer automatiquement les annonces expirées

- **Contexte :** En tant que plateforme, je veux retirer automatiquement les annonces obsolètes du catalogue, afin de ne pas indexer, charger ni servir des données inutiles en fin de vie d'une annonce.
- **Objectif :** Mettre en place un filtre serveur ou une tâche planifiée (ex. date d'expiration à **60 jours**) excluant **100 % des annonces expirées** du listing et de la recherche → viser une **réduction du nombre de produits servis et du poids de la réponse `/api/products`**.
- **Bonne pratique d'éco-conception ciblée :** Mettre en place une stratégie d'archivage et de suppression, automatique ou manuelle, des contenus obsolètes ou périmés, pour alléger les bases de données et les serveurs (RGESN 2024 — thème Contenus, critère 5.8 ; complément possible : critère 7.2 « durées de conservation des données »).
- **KPI associé :** Nombre de produits servis par `GET /api/products` et poids de la réponse, avant / après filtrage des expirés.
- **Repo / écran concerné :** Listing catalogue / recherche / filtrage serveur des annonces (parcours catalogue → fiche produit).
- **Critère de réussite :** Les annonces expirées ne sont plus renvoyées par l'API ni affichées dans le listing → volume de données servies réduit.
- **Niveau de priorité :** Moyenne.
- **Planification (roadmap 6 mois) :** M4 (suppression auto des annonces expirées).

---
