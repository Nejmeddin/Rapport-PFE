# 1.3 Identification des Acteurs & Cas d'Usage

### Analyse détaillée des acteurs

Il ne suffit pas de lister les acteurs. Il faut comprendre **le rôle exact de chacun**, ses besoins, ses contraintes, et comment il interagit avec le système :

**Acteur 1 — Le Gestionnaire Produit (Product Manager)**

- C'est l'utilisateur principal du dashboard
- Il ne maîtrise pas nécessairement la technique
- Il connaît bien les produits et peut juger si un attribut généré est correct
- Son besoin : voir rapidement les suggestions de l'IA, les valider en masse ou une par une, corriger si besoin, et avoir confiance dans les scores de fiabilité affichés
- Sa contrainte : il a peu de temps, le dashboard doit être intuitif et rapide

**Acteur 2 — Le Système LLM (IA multimodale)**

- C'est un acteur non-humain mais central
- Il reçoit en entrée : image produit + attributs existants + prompt structuré
- Il produit en sortie : un JSON structuré avec les attributs enrichis + un score de confiance
- Sa contrainte : il peut se tromper, halluciner, ou retourner des réponses incohérentes → d'où l'importance du pipeline de validation

**Acteur 3 — L'Administrateur SAP Hybris**

- Configure l'extension Hybris
- Gère les mappings entre attributs IA et modèle de données Hybris
- Supervise les imports et les rollbacks en cas d'erreur

**Acteur 4 — Apache Solr (acteur système)**

- Consomme les données enrichies après validation et injection dans Hybris
- Bénéficie de nouveaux champs pour améliorer les facettes et le ranking

**Acteur 5 — Le Client Final (acteur indirect)**

- N'interagit pas directement avec le système d'enrichissement
- Bénéficie du résultat : meilleure recherche, filtres plus précis, descriptions plus riches

### Modélisation des Cas d'Usage

**UC1 — Déclencher l'enrichissement d'un produit**

```markdown
Acteur principal : Système (automatique) ou Admin Hybris (manuel)
Précondition    : Le produit existe dans Hybris avec au moins une image
Flux principal  :
  1. Le système détecte un produit avec attributs incomplets
  2. Il récupère l'image et les attributs existants
  3. Il construit le prompt structuré
  4. Il appelle l'API LLM
  5. Il reçoit le JSON de réponse avec les attributs + scores
  6. Il stocke le résultat en état "En attente de validation"
Flux alternatif :
  4a. L'API LLM est indisponible → retry (3 tentatives) → mise en file d'attente
  5a. Le JSON retourné est malformé → parsing error → log + alerte admin
```

UC2 — Valider / Rejeter un attribut généré

```markdown
Acteur principal : Gestionnaire Produit
Précondition    : Des attributs sont en statut "En attente de validation"
Flux principal  :
  1. Le gestionnaire accède au dashboard
  2. Il voit la liste des produits avec attributs en attente
  3. Il sélectionne un produit et voit la comparaison avant/après
  4. Pour chaque attribut : il clique Valider, Rejeter ou Corriger
  5. Il soumet ses décisions
  6. Le système enregistre les décisions avec timestamp + identifiant utilisateur
Flux alternatif :
  4a. Le gestionnaire corrige une valeur → la valeur corrigée remplace
      la valeur IA et est marquée comme "validée manuellement"
  3a. Le score de confiance est très élevé (> 0.95) → suggestion de
      validation automatique en lot (bulk approve)
```

UC3 — Comparer avant/après enrichissement

```markdown
Acteur principal : Gestionnaire Produit
Précondition    : Un enrichissement a été effectué sur au moins un produit
                  (statut "En attente de validation" ou "Validé")
Flux principal  :
  1. Le gestionnaire accède au dashboard et sélectionne un produit
  2. Le système affiche une vue côte à côte (split view) :
       - Colonne gauche  : attributs AVANT enrichissement (données Hybris actuelles)
       - Colonne droite  : attributs APRÈS enrichissement (suggestions du LLM)
  3. Les attributs modifiés/ajoutés sont mis en évidence visuellement
     (surbrillance, icône, badge "Nouveau" ou "Modifié")
  4. Le score de confiance est affiché pour chaque attribut suggéré
  5. Le gestionnaire peut directement valider/rejeter depuis cette vue
     sans avoir à naviguer vers un autre écran
Flux alternatif :
  2a. Le produit n'a pas encore été enrichi → message informatif
      "Aucun enrichissement disponible pour ce produit"
  3a. Aucun attribut n'a changé (LLM a confirmé les valeurs existantes)
      → affichage d'un badge "Confirmé par l'IA" sur ces attributs
  4a. Score de confiance < 0.5 sur un attribut → mise en évidence
      en orange avec avertissement "Fiabilité faible, vérification recommandée"
```

UC4 — Importer les attributs validés dans Hybris

```markdown
Acteur principal : Extension SAP Hybris (automatique après validation)
Précondition    : Au moins un attribut est en statut "Validé"
Flux principal  :
  1. L'extension récupère les attributs validés via l'API interne
  2. Elle mappe chaque attribut IA vers le champ Hybris correspondant
  3. Elle génère un script ImpEx ou utilise le ServiceLayer Hybris
  4. Elle injecte les données dans la base Hybris
  5. Elle met à jour le statut de l'enrichissement à "Importé"
  6. Elle déclenche une réindexation partielle dans Solr
Flux alternatif :
  4a. Erreur d'injection → rollback + log + notification admin
  2a. Aucun mapping trouvé pour un attribut → log warning + attribut ignoré
```

UC5 — Indexer les attributs dans Solr

```markdown
Acteur principal : Système (automatique après import dans Hybris)
Acteur secondaire: Admin SAP Hybris (peut déclencher manuellement)
Précondition    : Des attributs validés ont été importés avec succès
                  dans SAP Hybris (UC3 complété)
Flux principal  :
  1. L'extension Hybris détecte la fin d'un import d'attributs validés
  2. Elle identifie les produits dont les données ont changé
  3. Elle déclenche une réindexation partielle (partial update)
     uniquement sur les produits modifiés, pour éviter une réindexation
     complète et coûteuse du catalogue
  4. Solr reçoit les nouveaux champs dynamiques et met à jour son index
  5. Les nouvelles facettes (couleur, matière, saisonnalité...) deviennent
     immédiatement disponibles pour la recherche utilisateur
  6. Le système log le nombre de documents réindexés + durée de l'opération
Flux alternatif :
  3a. Le nombre de produits modifiés dépasse un seuil configuré
      (ex : > 1000 produits) → basculement automatique en full reindex
      planifié la nuit pour ne pas impacter les performances
  4a. Erreur de connexion à Solr → retry (3 tentatives avec backoff
      exponentiel) → si toujours en échec : alerte admin + les produits
      concernés sont marqués "En attente de réindexation"
  2a. Un nouveau type d'attribut est indexé pour la première fois
      → vérification préalable que le champ dynamique correspondant
      existe dans le schéma Solr, sinon création automatique du champ
      avant l'indexation
```

UC6 — Consulter les indicateurs de qualité

```markdown
Acteur principal : Gestionnaire Produit / Admin
Flux principal  :
  1. Accès au tableau de bord KPIs
  2. Visualisation du taux de complétion global (avant vs après enrichissement)
  3. Visualisation du taux de validation manuelle vs automatique
  4. Visualisation de la distribution des scores de confiance par attribut
  5. Filtrage par catégorie de produit, période, attribut
```