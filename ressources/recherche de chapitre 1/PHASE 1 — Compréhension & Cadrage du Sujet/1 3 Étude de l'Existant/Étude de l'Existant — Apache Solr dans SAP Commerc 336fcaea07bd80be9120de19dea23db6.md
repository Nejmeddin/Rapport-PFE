# Étude de l'Existant — Apache Solr dans SAP Commerce

## Partie 1 — Définition, Fonctionnement & Limitations

### 1.1 Définition

Dans SAP Commerce Cloud, le moteur de recherche principalement utilisé est **Apache Solr**, un moteur open source basé sur **Apache Lucene**, optimisé pour l'indexation et la recherche full-text. Il est intégré nativement dans SAP Commerce pour gérer trois fonctions critiques : la recherche produit, les filtres de navigation (facettes) et l'auto-complétion.

Son rôle dans l'architecture est **complémentaire** à la base de données Hybris : là où Hybris stocke la source de vérité, Solr maintient un index optimisé pour répondre aux requêtes utilisateurs en quelques millisecondes.

### 1.2 Fonctionnement dans SAP Commerce

Solr agit comme un moteur d'indexation et de recherche externe. Le processus se déroule en 5 étapes : les données produits sont d'abord stockées dans la base SAP Commerce, puis un processus d'indexation les extrait et les transforme en documents Solr. Ces documents sont stockés dans l'index Solr. Lorsqu'un utilisateur effectue une recherche, la requête est envoyée au moteur Solr, qui retourne une liste de produits pertinents accompagnés de filtres (facettes).

```
Architecture Solr dans SAP Commerce :

┌─────────────────────────┐
│  Base de données Hybris │  ← Source de vérité
│  (Produits, Attributs)  │
└────────────┬────────────┘
             │ Processus d'indexation
             ▼
┌─────────────────────────┐
│      Index Solr         │  ← Optimisé pour la recherche
│  (Documents indexés)    │
└────────────┬────────────┘
             │ Requête utilisateur
             ▼
┌─────────────────────────┐
│  Résultats de recherche │
│  + Facettes (filtres)   │
└─────────────────────────┘
```

Ce modèle permet d'obtenir une recherche rapide et scalable sur de grands catalogues produits.

### 1.3 L'exemple Jacadi — Cas concret des limites de Solr

C'est l'exemple le plus parlant pour illustrer les défaillances réelles de Solr. Dans Jacadi, la recherche du mot **"table"** retourne des produits alors que le catalogue ne contient pas de tables. Ce n'est généralement pas un bug, mais un **effet du fonctionnement interne de Solr**.

Voici les mécanismes exacts qui expliquent ce comportement :

**Mécanisme 1 — La recherche Free Text**

Dans SAP Commerce, la recherche Solr est souvent configurée en **Free Text Search**, ce qui signifie que Solr ne cherche pas seulement dans `product.name`, mais aussi dans plusieurs champs indexés comme la description, les catégories, les tags, les attributs marketing, les labels, et d'autres champs indexés. Chaque Indexed Property peut être marquée comme `FreeTextSearch`, ce qui signifie qu'elle sera utilisée pour les recherches textuelles globales.

Résultat concret :

```
Produit : "Robe bébé"
Description : "Idéal pour dîner à table en famille"

→ Le mot "table" existe dans la description
→ Solr retourne ce produit ✅ (mais c'est non pertinent ❌)
```

**Mécanisme 2 — La Fuzzy Search**

Solr peut aussi utiliser une **recherche approximative**. Le terme "table" peut matcher des mots proches comme "tables", "tablet", "stable" ou "tableau". Donc un produit contenant le mot "tablette" peut apparaître dans les résultats.

**Mécanisme 3 — Les Synonymes configurés**

Dans SAP Commerce, Solr permet de définir des synonymes dans la configuration Backoffice. Si la config contient `table => desk` ou `table => furniture`, alors la requête "table" sera transformée en `table OR desk OR furniture`, ce qui peut faire remonter des chaises, bureaux ou meubles.

**Mécanisme 4 — La Tokenisation**

Solr découpe les mots en tokens. Le token `table` peut être contenu dans un mot comme `comfortable` si l'analyseur est mal configuré.

### 1.4 Limitations Complètes de Solr

Le document identifie **6 limitations structurelles** que tu dois maîtriser :

**Limitation 1 — Dépendance aux mots-clés (la principale)**

Solr fonctionne selon un modèle de recherche **lexical** : il analyse les tokens présents dans la requête et tente de les faire correspondre aux mots présents dans l'index. Il ne comprend pas réellement la signification ou l'intention derrière la requête.

```
Requête : "best vacuum for pet hair"
Solr analyse : [best] [vacuum] [pet] [hair]
→ Ne comprend pas : "aspirateur adapté aux poils d'animaux"
→ Peut retourner des produits non pertinents
```

**Limitation 2 — Absence de compréhension sémantique**

Contrairement aux moteurs modernes, Solr ne possède pas de mécanisme natif de recherche sémantique. La recherche sémantique permet de comprendre le contexte d'une requête, les relations entre les mots, et l'intention de l'utilisateur. Sans cette capacité, Solr ne peut pas gérer efficacement les requêtes complexes, les synonymes implicites et les formulations naturelles.

```
Requête : "running shoes for winter"
Solr    → matching lexical simple
Moteur sémantique → comprend "chaussures de course
                    adaptées aux conditions hivernales"
```

**Limitation 3 — Personnalisation limitée**

Un problème important est l'absence de personnalisation dynamique des résultats. Dans sa configuration standard, Solr retourne les **mêmes résultats pour tous les utilisateurs** effectuant la même recherche. Or les moteurs modernes doivent tenir compte de l'historique de navigation, l'historique d'achat, les préférences utilisateur et le comportement sur le site.

Les moteurs modernes utilisent des techniques de machine learning pour le ranking, alors que Solr nécessite généralement des **règles manuelles** pour ajuster la pertinence.

**Limitation 4 — Complexité de configuration et maintenance**

Améliorer la pertinence des résultats dans Solr nécessite souvent des interventions techniques importantes. Les équipes doivent configurer des règles de boosting, des synonymes, des analyzers linguistiques et des schémas d'indexation.

De plus, SAP Commerce utilise différents **Query Builders** pour transformer les requêtes utilisateur en requêtes Solr, ce qui peut rendre le comportement de la recherche difficile à analyser et à optimiser. Cette complexité augmente les coûts de maintenance et nécessite une expertise technique spécifique.

**Limitation 5 — Dépendance forte au processus d'indexation**

Le fonctionnement de Solr repose fortement sur l'indexation des données. Lors d'une **indexation complète (Full Index)**, l'index existant peut être supprimé et remplacé par un nouvel index, ce qui peut temporairement affecter la disponibilité de la recherche. Dans les catalogues comportant un grand nombre de produits, les opérations d'indexation peuvent être longues et coûteuses en ressources.

**Limitation 6 — Limitations structurelles dans SAP Commerce**

Dans certaines configurations, la recherche Solr dans SAP Commerce présente des limitations structurelles : la recherche est souvent conçue pour fonctionner principalement avec des catalogues produits, et la combinaison de différents types de données dans une seule recherche peut nécessiter des personnalisations importantes.

---

## Partie 2 — L'amélioration par LLM : Enrichissement des Attributs Produits et ses Limites

### 2.1 Ce que fait concrètement l'enrichissement LLM

Quand on utilise un LLM pour enrichir les attributs produits, on ajoute des **informations structurées supplémentaires** dans le catalogue. Par exemple, pour un produit "Robe bébé — Description : Robe coton rose", le LLM peut générer des attributs comme type, genre, couleur, matière, occasion, saison et style.

```
AVANT enrichissement LLM :
┌─────────────────────────────────┐
│ Produit : Robe bébé             │
│ Description : Robe coton rose   │
│ Attributs indexés dans Solr :   │
│   - name: "Robe bébé"           │
│   - description: "coton rose"   │
└─────────────────────────────────┘

APRÈS enrichissement LLM :
┌─────────────────────────────────┐
│ Produit : Robe bébé             │
│ Attributs enrichis dans Solr :  │
│   - type: robe                  │
│   - genre: bébé                 │
│   - couleur: rose               │
│   - matière: coton              │
│   - occasion: casual            │
│   - saison: été                 │
│   - style: enfant               │
└─────────────────────────────────┘
```

Cela permet d'avoir plus de filtres (facettes), une meilleure navigation et une meilleure correspondance avec des requêtes précises. Les systèmes modernes utilisent l'IA pour extraire et générer automatiquement des attributs produits à partir des descriptions, images ou autres données du catalogue.

De plus, un catalogue riche en attributs aide les moteurs de recherche à mieux faire correspondre les requêtes des utilisateurs et à réduire les résultats non pertinents.

**Impact de l'enrichissement selon le type de requête :**

| Cas d'usage | Impact de l'enrichissement |
| --- | --- |
| Requêtes longues ("robe bébé coton rose été") | ✅ Amélioration forte |
| Filtres et facettes | ✅ Amélioration forte |
| Navigation par catégorie | ✅ Amélioration forte |
| Compréhension du sens | ❌ Faible |

### 2.2 Pourquoi l'enrichissement seul ne suffit pas — Les limites fondamentales

C'est le point le plus important de cette partie, et il justifie directement la solution complète de ton PFE.

La réponse honnête est : **non, utiliser un LLM uniquement pour enrichir les attributs des produits ne suffit généralement pas à résoudre le problème**. Cela peut améliorer la recherche, mais ne corrige pas la limite fondamentale de Solr.

**Pourquoi ? La logique de Solr ne change pas.**

Même avec des attributs enrichis, Solr reste **un moteur de recherche lexical (keyword-based)**. Cela signifie que pour une requête `"table"`, Solr va chercher le mot "table" dans l'index. Si le mot existe dans la description, les synonymes, un attribut marketing ou un texte SEO, il retourne le produit — même si ce produit est un vêtement.

**Illustration du problème persistant avec Jacadi :**

Dans le cas Jacadi avec `query = "table"` et `catalogue = vêtements`, un moteur lexical raisonne ainsi : "table existe dans un texte ? → oui → retourner produit". En revanche, un moteur sémantique raisonne : "table = meuble, catalogue = vêtements → pas de correspondance → 0 résultats". C'est exactement ce qu'on observe avec Velou.

```
┌──────────────────────────────────────────────────┐
│  LIMITE DE L'ENRICHISSEMENT SEUL                 │
│                                                  │
│  Fiche produit enrichie :                        │
│    type=robe, couleur=rose, saison=été...        │
│    description="idéal à table en famille"       │
│                                                  │
│  Requête utilisateur : "table"                  │
│                                                  │
│  Comportement Solr :                             │
│    → Cherche "table" dans TOUS les champs       │
│    → Trouve "table" dans description            │
│    → Retourne la robe ❌                         │
│                                                  │
│  L'enrichissement n'a rien changé               │
│  au comportement lexical de Solr !              │
└──────────────────────────────────────────────────┘
```

Cette limite est clairement identifiée : l'enrichissement seul reste du matching lexical — même avec de nouveaux attributs, le moteur n'ajuste pas l'intention de recherche.

---

## Partie 3 — La Solution Complète avec les LLMs

Pour vraiment résoudre les limitations de Solr, il faut une architecture à plusieurs niveaux. Une approche prometteuse consiste à **augmenter Solr avec une couche d'IA**, plutôt que de remplacer entièrement l'infrastructure existante. Cette architecture hybride peut inclure un modèle de langage (LLM), une base de données vectorielle, un système de recherche sémantique et un mécanisme de recommandation intelligent.

### 3.1 Composant 1 — Query Understanding avec LLM

Ce qui change vraiment la donne est de faire **interpréter la requête utilisateur par un LLM avant de l'envoyer à Solr**. Le LLM analyse la requête en langage naturel et la transforme en contraintes structurées exploitables.

**Exemple concret :**

Pour la requête "robe de soirée élégante pour mariage", le LLM analyse et transforme cela en un objet JSON structuré contenant l'intent, la catégorie cible et les filtres (type: robe, occasion: mariage, style: élégant). Ce JSON est ensuite converti en requête Solr avec filtres, ce qui donne une bien meilleure pertinence.

```json
// Requête utilisateur → LLM → JSON structuré → Solr

Entrée  : "robe de soirée élégante pour mariage"

Sortie LLM :
{
  "intent": "recherche produit",
  "target_category": "vêtements",
  "filters": {
    "type": "robe",
    "occasion": "mariage",
    "style": "élégant"
  }
}

→ Requête Solr générée :
   q=robe&fq=occasion:mariage&fq=style:élégant
```

**Résolution du cas Jacadi :**

```
Requête : "table"
LLM → category = furniture
catalogue = clothing
→ Solr reçoit un filtre category=furniture
→ 0 résultats ✅ (correct !)
```

### 3.2 Composant 2 — Recherche Sémantique (Vector Search)

Les LLM peuvent aussi générer des **vecteurs de sens** : la requête utilisateur est transformée en vecteur, et chaque produit est aussi représenté par un vecteur. On effectue ensuite une recherche vectorielle pour trouver les produits "proches" en sens — ce qui corrige le problème de Solr qui reste lexical.

La mise en place se fait en 4 étapes : pour chaque produit, on génère un vecteur via un modèle LLM ou spécialisé ; on stocke ces vecteurs dans une base dédiée (Faiss, Pinecone, etc.) ; à la recherche, on transforme la requête en vecteur ; puis on recherche les produits les plus similaires via une mesure de distance cosinus pour obtenir des résultats sémantiques plutôt que lexicaux.

```
Produits → Vecteurs (embeddings)
┌──────────────────────────────────────┐
│ "Robe bébé coton rose" → [0.2, 0.8, 0.1, ...]  │
│ "Pyjama enfant hiver"  → [0.1, 0.7, 0.3, ...]  │
│ "Veste imperméable"    → [0.3, 0.2, 0.9, ...]  │
└──────────────────────────────────────┘

Requête "tenue légère pour bébé" → [0.2, 0.75, 0.15, ...]
→ Similarité cosinus → "Robe bébé coton rose" ✅
```

Cette étape n'a pas besoin de remplacer Solr : elle sert à ajouter une **couche parallèle**.

### 3.3 Composant 3 — L'Architecture Hybride Complète (Recommandée)

Pour une solution robuste, l'architecture moderne combine LLM Intent Understanding, Vector Search, Solr Retrieval et un Re-ranking hybride.

```
┌─────────────────────────────────────────────────────────┐
│              ARCHITECTURE HYBRIDE COMPLÈTE              │
│                                                         │
│  Utilisateur                                            │
│       │                                                 │
│       ▼                                                 │
│  ┌─────────────────────────┐                            │
│  │  1) LLM Intent          │ ← Comprend l'intention    │
│  │     Understanding       │   Extrait filtres/catég.  │
│  └────────────┬────────────┘                            │
│               │                                         │
│       ┌───────┴────────┐                                │
│       ▼                ▼                                │
│  ┌──────────┐   ┌──────────────┐                        │
│  │ 2) Vector│   │ 3) Solr      │ ← Les deux en          │
│  │   Search │   │   Retrieval  │   parallèle            │
│  │(sémantic)│   │ (keyword+filtre)                      │
│  └──────────┘   └──────────────┘                        │
│       │                │                                │
│       └───────┬─────────┘                               │
│               ▼                                         │
│  ┌─────────────────────────┐                            │
│  │  4) Hybrid Merge &      │ ← Combine et ordonne      │
│  │     Re-ranking (LLM)    │   les résultats           │
│  └────────────┬────────────┘                            │
│               │                                         │
│               ▼                                         │
│       Search Results ✅                                 │
└─────────────────────────────────────────────────────────┘
```

**Rôle de chaque composant :**

Le tableau des rôles est le suivant : la Compréhension est assurée par le LLM (décoder intention + filtres), la Récupération sémantique par le Vector Search (trouver produits par sens), la Récupération classique par Solr (matching filtré), le Re-ranking par le LLM ranking (combiner et ordonner) et le Résultat est présenté dans l'UI.

### 3.4 Bénéfices Concrets de la Solution Complète

La solution hybride résout 4 catégories de problèmes : pour les requêtes ambiguës, le LLM comprend l'intention plutôt que le mot brut ; pour les typos ou phrases maladroites, le Vector Search trouve les bons produits au sens proche ; pour les recherches multi-critères, le LLM distribue les contraintes (type/occasion/style) et les applique au moteur ; pour la recherche longue traîne, le LLM transforme en filtres exploitables par Solr et la recherche vectorielle.

**Résolution définitive du cas Jacadi :**

Avant la solution hybride, la requête "table" via Solr matchait une description et retournait des produits non pertinents. Après, le LLM identifie que "table" est un terme non relié au catalogue vêtement, et Solr combiné aux vecteurs ne retourne rien — résultat correct.

### 3.5 Bonnes Pratiques d'Implémentation

Quatre bonnes pratiques sont essentielles : d'abord **structurer les données produits** (standardiser titres et descriptions avant de générer du sens) ; ensuite **construire les embeddings produits** (générer un vecteur par produit pour le semantic search) ; puis **construire un pipeline LLM pour les requêtes** (transformer les recherches utilisateurs en slots structurés) ; et enfin **gérer le cache et la latence** (un LLM en temps réel peut être coûteux → utiliser cache + génération offline pour les requêtes fréquentes).

### 3.6 Tableau de Synthèse Final

| Approche | Ce qu'elle résout | Ce qu'elle ne résout pas | Efficacité |
| --- | --- | --- | --- |
| **Solr seul** | Recherche rapide, facettes basiques | Intention, sémantique, personnalisation | ⭐⭐ |
| **Solr + Enrichissement LLM** | Facettes riches, filtres précis, requêtes longues | Requêtes ambiguës, compréhension du sens | ⭐⭐⭐ |
| **Query Understanding LLM** | Intention utilisateur, filtres automatiques | Typos, reformulations | ⭐⭐⭐⭐ |
| **Vector Search** | Sémantique, similarité de sens | Latence, coût infrastructure | ⭐⭐⭐⭐⭐ |
| **Hybrid LLM + Solr** | Tout ce qui précède | Coût de maintien à jour | ⭐⭐⭐⭐⭐ |

### 3.7 Positionnement de ton PFE dans cette architecture

> **Point clé pour ton mémoire** : Ton projet se positionne sur la **brique fondamentale** de cette architecture — l'enrichissement des attributs produits. Sans cette brique, ni le Vector Search ni le Query Understanding ne peuvent fonctionner correctement, car ils ont besoin de **données produit riches et structurées** comme base. Tu construis donc le **socle indispensable** à toute amélioration ultérieure de la recherche chez DECADE.
> 

```
Ce que tu construis dans ton PFE :
        ┌───────────────────────────────┐
        │  LLM Multimodal               │
        │  ↓                            │
        │  Attributs enrichis           │◄── TON PROJET
        │  (couleur, matière, style...) │
        │  ↓                            │
        │  SAP Hybris (validés)         │
        │  ↓                            │
        └──────────┬────────────────────┘
                   │
        ┌──────────▼────────────────────┐
        │  Solr — Index enrichi         │◄── Bénéficiaire direct
        │  Facettes dynamiques          │
        │  Ranking amélioré             │
        └───────────────────────────────┘
                   │
        ┌──────────▼────────────────────┐
        │  (Perspectives futures)       │
        │  Vector Search                │◄── Rendu possible
        │  Query Understanding LLM      │    par ton travail
        └───────────────────────────────┘
```

---

> **À retenir pour la soutenance** : Tu peux démontrer que tu maîtrises non seulement ce que tu as implémenté, mais aussi le contexte architectural plus large dans lequel ton projet s'inscrit. L'enrichissement des attributs n'est pas une fin en soi — c'est la condition préalable à une recherche vraiment intelligente.
>