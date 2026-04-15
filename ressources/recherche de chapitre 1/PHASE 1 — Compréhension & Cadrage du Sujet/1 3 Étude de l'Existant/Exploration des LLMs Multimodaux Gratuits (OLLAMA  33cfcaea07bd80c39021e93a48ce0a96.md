# Exploration des LLMs Multimodaux Gratuits (OLLAMA ,OPENROUTER, GROQ)

## 🏷️ **ÉTUDE DE L'EXISTANT — PROJET DECADE**

*Évaluation comparative dans le cadre de l'enrichissement automatique des attributs produit*

---

**Contexte du projet :** L'objectif est de concevoir une solution d'enrichissement automatique des données produit à partir d'images, de descriptions textuelles et d'attributs existants, en s'appuyant sur un LLM multimodal (Large Language Model capable de traiter simultanément du texte et des images). Avant de retenir un modèle, une phase d'exploration des solutions gratuites a été menée selon trois axes : exécution locale, APIs cloud via OpenRouter, et APIs cloud via Groq.

---

## 1. Exécution locale via Ollama

**Qu'est-ce qu'Ollama ?**
Ollama est un outil open source permettant d'exécuter des modèles de langage directement sur une machine locale, sans avoir besoin de passer par une API externe. Il offre une interface simple pour télécharger et interroger des LLMs multimodaux en local, qui présentent l'avantage de la confidentialité des données et de l'absence de modèle de requêtes.

Les modèles suivants ont été testés en local pour leur capacité à traiter des images de produits accompagnées de métadonnées textuelles :

### Gemma 4

- 🟢 `Google DeepMind`
- Téléchargement : ✅
- Inférence image : sature le CPU → GPU
- Utilisable : ❌

### Qwen-VL

- 🔵 `Alibaba`
- Téléchargement : ✅
- Inférence image : sature le CPU → GPU
- Utilisable : ❌

### Moondream

- 🟢 `Open source`
- Téléchargement : ✅
- Inférence image : trop lent / non performant
- Utilisable : ❌

> ⚠️ **Verdict — Non viable :** L'exécution locale de modèles multimodaux requiert un **GPU dédié** (VRAM ≥ 8 Go recommandée). En l'absence de GPU, le traitement repose entièrement sur le CPU, ce qui rend les temps d'inférence prohibitifs. De plus, les images encodées en base64 génèrent un volume de tokens très élevé, ce qui sature rapidement la mémoire vive de la machine et provoque des plantages.
> 

---

## 2. APIs gratuites via OpenRouter

**Qu'est-ce qu'OpenRouter ?**
OpenRouter est une plateforme d'agrégation d'APIs de modèles de langage. Elle permet d'accéder à de nombreux modèles (propriétaires et open source) via une interface unifiée. Certains modèles y sont disponibles gratuitement avec des quotas limités sur la taille du contexte (nombre de tokens en entrée) et le débit de requêtes.

Trois niveaux de complexité d'input ont été testés pour chaque modèle, représentant les cas d'usage réels du projet :

### Gemma 4

- 🟢 `Google` · `Free tier`
- 1 image : ✅reussi
- texte : ✅reussi
- Images multiples : ❌context overflow
- Images + Métadonnées : ❌context overflow

### Nemotron Nano 12B 2 VL

- 🟣 `NVIDIA` · `Free tier`
- 1 image : ✅reussi
- texte : ✅reussi
- Images multiples : ✅reussi
- Images + Métadonnées : ❌context overflow

### Qwen 3.6 Plus

- 🔵 `Alibaba` · `Free tier`
- 1 image : ❌context overflow
- texte : ✅reussi
- Images multiples : ❌context overflow
- Images + Métadonnées : ❌context overflow

> ⚠️ **Verdict — Partiellement viable, non robuste :** Les modèles du tier gratuit imposent une **fenêtre de contexte réduite**. Le cas d'usage complet du projet — plusieurs images produit encodées + attributs textuels + descriptif — dépasse systématiquement les limites allouées. Qwen 3.6 Plus a de plus rencontré des erreurs HTTP 429 dès les premiers appels, indiquant une saturation de l'upstream Alibaba côté OpenRouter.
> 

---

## 3. APIs gratuites via Groq

**Qu'est-ce que Groq ?**
Groq est une plateforme d'inférence ultra-rapide basée sur des puces LPU (Language Processing Unit) propriétaires. Elle propose une API gratuite donnant accès à des modèles comme LLaMA 3 avec des vitesses de génération impressionnantes. Cependant, le tier gratuit impose des quotas stricts sur le nombre de tokens par minute (TPM — **Tokens Per Minute**).

Les modèles **llama-3.3-70b-versatile** et **groq/compound** ont été testés avec un input représentatif d'une fiche produit réelle.

| LIMITE GRATUITE | INPUT D'UN PRODUIT (AVEC LE MINIMUM DE METADATA) | DÉPASSEMENT |
| --- | --- | --- |
| **12 000** tokens / minute | **46 214** tokens reçus | **×3,8** au-delà du quota |

**Erreur retournée — HTTP 413 :**

```
Error 413: Request Too Large for model 'llama-3.3-70b-versatile'
Limit 12,000 TPM | Requested 46,214 tokens
→ rate_limit_exceeded
```

> ❌ **Verdict — Non viable :** Même un seul produit avec ses images et attributs dépasse largement le quota TPM du tier gratuit. Le passage au **Dev Tier payant** (proposé par Groq) serait nécessaire pour toute utilisation en production. Cette option sort du cadre de l'étude des solutions gratuites.
> 

---

## 4. Synthèse comparative

| APPROCHE | PLATEFORME | MULTIMODAL | VERDICT |
| --- | --- | --- | --- |
| Local | Ollama | ✅ | 🔴 Machine insuffisante |
| API cloud | OpenRouter | ✅ | 🟡 Quota souvent trop faible |
| API cloud | Groq | ✅ | 🔴 Limite TPM dépassée ×3,8 |

> 💡 La contrainte fondamentale commune à toutes les approches est le **volume d'input** : un pipeline d'enrichissement produit réaliste génère un contexte bien supérieur aux quotas gratuits disponibles, que ce soit en mémoire GPU (local) ou en tokens alloués par minute (cloud).
> 

---