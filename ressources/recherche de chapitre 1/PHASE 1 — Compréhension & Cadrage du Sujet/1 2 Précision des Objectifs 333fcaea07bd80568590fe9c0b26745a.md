# 1.2 Précision des Objectifs

La précision des objectifs, c'est la **boussole du projet**. Un objectif mal formulé mène à un développement mal orienté, et à une soutenance où tu ne peux pas démontrer ce que tu as accompli.

### Comment formuler un bon objectif : la méthode SMART

Chaque objectif doit être **Spécifique, Mesurable, Atteignable, Réaliste, Temporellement borné** :

[amelioration des objectif avec la methode SMART](1%202%20Pr%C3%A9cision%20des%20Objectifs/amelioration%20des%20objectif%20avec%20la%20methode%20SMART%20333fcaea07bd800cb341ed34e2732d0a.csv)

### Hiérarchie des objectifs à formaliser

**Objectif principal** (1 seul, central) :

> Réduire le temps et le coût d'enrichissement des données produit en automatisant l'extraction d'attributs via un LLM multimodal, tout en garantissant la qualité via un pipeline de validation humaine, dans un environnement SAP Hybris + Apache Solr.
> 

**Objectifs secondaires** (décomposition du principal, 4 à 6 max) :

```markdown
OS1 — Automatisation de l'enrichissement
→ Le LLM doit extraire correctement au moins 6 types d'attributs
à partir d'une image et/ou d'une description textuelle existante
```

```markdown
OS2 — Contrôle qualité humain
→ Le dashboard doit permettre la validation, correction et rejet
des attributs générés, avec un audit trail complet
```

```markdown
OS3 — Intégration native à Hybris
→ L'extension Hybris doit mapper et injecter automatiquement
les attributs validés dans le modèle de données existant
```

```markdown
OS4 — Amélioration de la recherche Solr
→ Les nouveaux attributs doivent alimenter les facettes dynamiques
et améliorer le ranking des résultats de recherche
```

```markdown
OS5 — Exposition via API
→ Une API REST documentée doit exposer les données enrichies
pour permettre la consommation par des systèmes tiers
```

```markdown
OS6 — (Optionnel/secondaire) Gestion multilingue
→ Les descriptions enrichies doivent être générables en FR, EN, DE
```

**Objectifs de qualité** (non fonctionnels) :

> 
> 
> - **Performance** : l'enrichissement d'un produit (appel LLM + structuration) doit prendre < 5 secondes en mode synchrone, ou être traitable en batch à raison de X produits/heure
> - **Fiabilité** : mécanisme de retry et de fallback en cas d'échec de l'API LLM
> - **Auditabilité** : chaque modification doit être tracée (qui a validé, quand, quelle valeur avant/après)
> - **Maintenabilité** : l'ajout d'un nouveau type d'attribut ne doit pas nécessiter de refactoring majeur

### Délimitation claire des priorités

Tout ne peut pas être fait au même niveau de finition. Classer les objectifs en 3 catégories :

```markdown
MUST HAVE (cœur du projet, indispensable pour la soutenance) :
  ✅ Enrichissement LLM (au moins image + texte → 6 attributs)
  ✅ Dashboard de validation fonctionnel
  ✅ Extension Hybris (import des attributs validés)
  ✅ Indexation Solr de base

SHOULD HAVE (important, à faire si le temps le permet) :
  🔶 Détection d'anomalies et non-conformités
  🔶 Score de confiance par attribut
  🔶 API REST documentée (Swagger)

NICE TO HAVE (bonus, à mentionner en perspectives) :
  💡 Gestion multilingue complète
  💡 Enrichissement en temps réel (webhook à la création produit)
  💡 Recommandations de recatégorisation automatique
```