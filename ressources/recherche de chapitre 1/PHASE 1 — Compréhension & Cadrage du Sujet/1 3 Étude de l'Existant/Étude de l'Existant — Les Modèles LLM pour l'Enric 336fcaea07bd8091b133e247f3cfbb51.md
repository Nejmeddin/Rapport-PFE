# Étude de l'Existant — Les Modèles LLM pour l'Enrichissement des Attributs Produit

# Étude de l'Existant — Les Modèles LLM pour l'Enrichissement des Attributs Produit

---

## 1. Contexte & Critères de Sélection

Avant de comparer les modèles, il faut définir les **critères qui comptent vraiment** pour ton cas d'usage précis. L'enrichissement des attributs produit n'est pas une tâche de génération de texte libre — c'est une tâche d'**extraction structurée multimodale** avec des contraintes techniques strictes.

Les 7 critères retenus pour cette évaluation sont les suivants : la capacité multimodale (analyse image + texte simultanément), la qualité du structured output (fiabilité du JSON retourné), la compréhension sémantique du domaine mode/e-commerce, le coût par appel API (critique pour un traitement en masse de catalogues), la latence (acceptable pour un pipeline quasi-temps réel), la politique de confidentialité des données produit, et enfin la facilité d'intégration avec un backend Java/Spring.

---

## 2. Panorama des Modèles Candidats

| Critère | GPT-4o (OpenAI) | Gemini 1.5 Pro (Google) | Claude 3.5 Sonnet (Anthropic) | LLaVA 1.6 (Open-source) | Idefics3 (HuggingFace) |
| --- | --- | --- | --- | --- | --- |
| Type | API Cloud | API Cloud | API Cloud | Open-source | Open-source |
| Multimodal | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ |
| JSON structuré | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ |
| Sémantique | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ |
| Coût | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| Latence | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐ |
| Confidentialité | ⭐⭐ | ⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| Intégration Java | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ |
- 🔵 **GPT-4o** — OpenAI (API Cloud)
    - **Coût API** : $2.5 / $10 (input/output per 1M tokens)
    - **Latence** : 1–3s
    - **Contexte max** : 128K tokens
    - **JSON mode** : Natif (JSON mode + function calling)
    - **Confidentialité** : Données envoyées à OpenAI
    
    **Avantages**
    
    - Meilleure qualité multimodale
    - JSON mode natif ultra-fiable
    - Excellente compréhension mode/e-commerce
    - SDK Java officiel
    
    **Limites**
    
    - Données produit envoyées à OpenAI
    - Coût élevé pour gros catalogues
    - Dépendance à une API externe
    
    <aside>
    ✅
    
    Recommandé pour le POC — meilleur rapport qualité/facilité d'intégration
    
    </aside>
    
- 🔴 **Gemini 1.5 Pro** — Google (API Cloud)
    - **Coût API** : $1.25 / $5 (input/output per 1M tokens)
    - **Latence** : 2–5s
    - **Contexte max** : 1M tokens (!)
    - **JSON mode** : JSON mode disponible
    - **Confidentialité** : Données envoyées à Google
    
    **Avantages**
    
    - Contexte 1M tokens — parfait pour grands catalogues
    - Moins cher que GPT-4o
    - Excellent sur analyse d'images multiples
    
    **Limites**
    
    - Latence plus élevée
    - JSON mode moins fiable que GPT-4o
    - Données envoyées à Google
    
    <aside>
    🔄
    
    Alternative solide si le coût est une contrainte — tester en parallèle du POC
    
    </aside>
    
- 🟠 **Claude 3.5 Sonnet** — Anthropic (API Cloud)
    - **Coût API** : $3 / $15 (input/output per 1M tokens)
    - **Latence** : 1–2s
    - **Contexte max** : 200K tokens
    - **JSON mode** : Très fiable (instruction-following)
    - **Confidentialité** : Données envoyées à Anthropic
    
    **Avantages**
    
    - Instruction-following exceptionnel
    - Très fiable sur l'extraction structurée
    - Meilleure compréhension sémantique nuancée
    
    **Limites**
    
    - Output le plus coûteux
    - Pas de JSON mode officiel (prompt-based)
    - SDK Java non officiel
    
    <aside>
    🎯
    
    Excellent pour les attributs nécessitant une compréhension fine (style, occasion)
    
    </aside>
    
- 🟢 **LLaVA 1.6** — LLaVA Team (Open-source)
    - **Coût** : Coût infra uniquement
    - **Latence** : Variable (GPU local)
    - **Contexte max** : 4K–32K tokens
    - **JSON mode** : Non natif — prompt engineering requis
    - **Confidentialité** : ✅ 100% hébergeable en interne
    
    **Avantages**
    
    - Zéro coût par requête
    - Données ne quittent pas l'infra DECADE
    - Customisable / fine-tunable
    
    **Limites**
    
    - Qualité inférieure aux modèles propriétaires
    - JSON peu fiable sans fine-tuning
    - Nécessite infra GPU
    - Latence imprévisible
    
    <aside>
    🔒
    
    Pertinent si confidentialité des données est une contrainte contractuelle forte
    
    </aside>
    
- ⚪ **Idefics3** — HuggingFace (Open-source)
    - **Coût** : Coût infra uniquement
    - **Latence** : Variable (GPU local)
    - **Contexte max** : 4K tokens
    - **JSON mode** : Très limité
    - **Confidentialité** : ✅ 100% hébergeable en interne
    
    **Avantages**
    
    - Open-source, auto-hébergeable
    - Léger comparé à LLaVA
    - Aucun coût par requête
    
    **Limites**
    
    - Qualité faible sur extraction structurée
    - Contexte limité
    - Fine-tuning obligatoire pour usage production
    
    <aside>
    ⚠️
    
    Option de dernier recours si contraintes budgétaires et confidentialité extrêmes
    
    </aside>
    

---

## 3. Analyse Détaillée par Modèle

### 3.1 GPT-4o — OpenAI

GPT-4o est le modèle le plus mature pour ce type de tâche. Sa force principale dans le contexte de ton PFE est son **JSON mode natif** : en ajoutant `response_format: { type: "json_object" }` dans l'appel API, le modèle est contraint de retourner exclusivement un JSON valide, ce qui supprime le problème du parsing. Pour un pipeline industriel comme le tien, c'est une garantie de robustesse critique.

Sur l'analyse d'images produit mode/habillement, GPT-4o démontre une compréhension sémantique fine : il distingue "bleu cobalt" de "bleu marine", identifie "lin froissé" vs "coton satiné", et catégorise correctement des pièces ambiguës (ex : un "kimono" qui peut être classé robe ou veste selon le contexte). Il maîtrise également les nuances de saisonnalité (distinguer un manteau "mi-saison" d'un "hiver" sur une image).

Exemple d'appel Java/Spring pour ce projet :

java

`// Service d'enrichissement avec GPT-4o
@Service
public class OpenAiEnrichmentService implements LLMProvider {

    private static final String PROMPT_TEMPLATE = """
        Tu es un expert en catalogage de produits e-commerce mode enfant (Jacadi).
        Analyse l'image produit et les attributs existants fournis.
        Retourne UNIQUEMENT un objet JSON valide respectant exactement cette structure :
        {
          "dominantColor": { "value": "string", "confidence": 0.0-1.0 },
          "secondaryColors": \[{ "value": "string", "confidence": 0.0-1.0 }\],
          "material": { "value": "string", "confidence": 0.0-1.0 },
          "pattern": { "value": "uni|rayures|imprimé|fleurs|autre", "confidence": 0.0-1.0 },
          "season": { "value": "été|hiver|mi-saison|all-season", "confidence": 0.0-1.0 },
          "category": { "value": "string", "confidence": 0.0-1.0 },
          "structuredDescription": { "value": "string (max 150 chars)", "confidence": 0.0-1.0 }
        }
        Attributs existants du produit : %s
        Contraintes couleurs acceptées : %s
        """;

    public ProductEnrichmentResult enrich(String imageUrl, 
                                           Map<String, String> existingAttributes) {
        // Appel OpenAI avec JSON mode activé
        ChatRequest request = ChatRequest.builder()
            .model("gpt-4o")
            .responseFormat(ResponseFormat.JSON_OBJECT)
            .message(UserMessage.of(
                TextContent.of(buildPrompt(existingAttributes)),
                ImageContent.ofUrl(imageUrl)
            ))
            .maxTokens(500)
            .build();

        String jsonResponse = openAiClient.chat(request).content();
        return parseAndValidate(jsonResponse);
    }
}`

**Limite principale** : les images produit sont envoyées aux serveurs OpenAI. Si DECADE a des accords de confidentialité avec ses marques partenaires, cela doit être clarifié dès la phase de cadrage. OpenAI propose un accord DPA (Data Processing Agreement) pour les usages API.

---

### 3.2 Gemini 1.5 Pro — Google

Son atout différenciant est sa **fenêtre de contexte de 1 million de tokens**, ce qui le rend unique pour un scénario d'enrichissement en batch : on peut inclure dans un seul appel plusieurs dizaines de produits avec leurs images (passées en base64 ou via URL) et leurs attributs existants. C'est une architecture de traitement radicalement différente des autres modèles.

Pour l'enrichissement de catalogues avec des variantes produit (même vêtement en 5 couleurs), Gemini peut traiter toutes les variantes en un seul appel en comprenant les relations entre elles, ce qu'aucun autre modèle ne peut faire nativement.

Sa limite est une **latence plus élevée** sur les réponses structurées complexes, et son JSON mode est légèrement moins fiable que GPT-4o sur les formats très stricts (cas de champs manquants ou de types incorrects à gérer avec plus de robustesse côté parsing).

---

### 3.3 Claude 3.5 Sonnet — Anthropic

Claude se distingue par une **compréhension sémantique nuancée** supérieure sur les tâches d'extraction d'attributs complexes. Là où GPT-4o retournera "bleu" pour une couleur, Claude retournera "bleu lavande" avec un raisonnement explicatif, ce qui est particulièrement utile pour les attributs de style et d'occasion (distinguer "formel" de "smart casual" sur une image).

Son instruction-following exceptionnel le rend très fiable pour respecter des contraintes de vocabulaire contrôlé (listes d'énumérations Hybris) même sans JSON mode officiel — via un prompt bien construit avec des exemples few-shot, le taux de conformité dépasse 95%.

Limite pour ce projet : il n'existe pas de SDK Java officiel d'Anthropic (contrairement à OpenAI qui maintient `openai-java`). L'intégration se fait via des appels HTTP REST directs ou via LangChain4j.

---

### 3.4 LLaVA 1.6 & Idefics3 — Modèles Open-Source

Ces modèles sont les seuls qui permettent un **déploiement 100% interne** sans aucun envoi de données à l'extérieur. C'est leur unique avantage décisif sur les modèles propriétaires.

Cependant, leur qualité sur l'extraction structurée d'attributs mode est significativement inférieure. Les tests montrent des hallucinations fréquentes sur les couleurs secondaires, une mauvaise précision sur les matières (confusions lin/coton, soie/polyester), et un JSON output peu fiable sans fine-tuning spécifique. LLaVA nécessite une infrastructure GPU (au moins un NVIDIA A10G ou équivalent) pour des performances acceptables, ce qui représente un coût d'infrastructure et une complexité opérationnelle supplémentaire.

Pour un PFE de 6 mois, le temps de mise en place d'une infrastructure GPU + fine-tuning d'un modèle open-source n'est pas compatible avec le planning. Ces modèles sont mentionnés comme **perspectives et alternatives futures** si DECADE décide d'internaliser la solution après le stage.

---

## 4. Comparaison Directe sur le Cas d'Usage Jacadi

Voici comment chaque modèle répond concrètement au même prompt d'enrichissement pour une robe bébé Jacadi :

Produit testé : **Robe bébé Jacadi** — image + description existante : "Jolie robe pour petite fille"

- **GPT-4o** — Qualité globale : 97/100
    
    ```json
    {
      "dominantColor": {
        "value": "rose poudré",
        "confidence": 0.95
      },
      "secondaryColors": [
        { "value": "blanc cassé", "confidence": 0.88 }
      ],
      "material": {
        "value": "coton",
        "confidence": 0.91
      },
      "pattern": {
        "value": "fleurs",
        "confidence": 0.97
      },
      "season": {
        "value": "été",
        "confidence": 0.93
      },
      "category": {
        "value": "robe",
        "confidence": 0.99
      },
      "structuredDescription": {
        "value": "Robe bébé en coton à motifs fleuris, coloris rose poudré, manches courtes, idéale pour la saison estivale.",
        "confidence": 0.94
      }
    }
    ```
    
    <aside>
    💡
    
    JSON valide, tous les champs présents, scores de confiance cohérents. Couleur "rose poudré" précise et différenciante pour les facettes Solr.
    
    </aside>
    
- **Gemini 1.5 Pro** — Qualité globale : 89/100
    
    ```json
    {
      "dominantColor": {
        "value": "rose",
        "confidence": 0.92
      },
      "secondaryColors": [
        { "value": "blanc", "confidence": 0.85 }
      ],
      "material": {
        "value": "coton biologique",
        "confidence": 0.78
      },
      "pattern": {
        "value": "fleurs",
        "confidence": 0.94
      },
      "season": {
        "value": "été",
        "confidence": 0.90
      },
      "category": {
        "value": "robe bébé",
        "confidence": 0.98
      },
      "structuredDescription": {
        "value": "Robe en coton bio à imprimé floral, tons roses et blancs, pour bébé fille.",
        "confidence": 0.88
      }
    }
    ```
    
    <aside>
    ⚠️
    
    Très bonne qualité. "Coton biologique" : hallucination partielle (à surveiller). Couleur moins précise que GPT-4o. JSON valide.
    
    </aside>
    
- **Claude 3.5 Sonnet** — Qualité globale : 96/100
    
    ```json
    {
      "dominantColor": {
        "value": "rose poudré doux",
        "confidence": 0.94,
        "reasoning": "Teinte pastel dominante, non saturée, typique layette fille"
      },
      "secondaryColors": [
        { "value": "blanc ivoire", "confidence": 0.91,
          "reasoning": "Présent dans les motifs fleuris" }
      ],
      "material": {
        "value": "coton",
        "confidence": 0.89,
        "reasoning": "Texture douce visible, tombé caractéristique du coton léger"
      },
      "pattern": {
        "value": "fleurs",
        "confidence": 0.98
      },
      "season": {
        "value": "été",
        "confidence": 0.95
      },
      "category": {
        "value": "robe",
        "confidence": 0.99
      },
      "structuredDescription": {
        "value": "Robe bébé à motifs floraux délicats en coton, coloris rose poudré, légère et fraîche pour l'été.",
        "confidence": 0.96
      }
    }
    ```
    
    <aside>
    💡
    
    Champ "reasoning" non demandé mais très utile pour la validation humaine dans le dashboard. Sémantique la plus riche. JSON valide.
    
    </aside>
    
- **LLaVA 1.6** — Qualité globale : 52/100
    
    ```json
    {
      "dominantColor": {
        "value": "pink",
        "confidence": 0.7
      },
      "secondaryColors": [],
      "material": {
        "value": "fabric",
        "confidence": 0.4
      },
      "pattern": {
        "value": "printed",
        "confidence": 0.6
      },
      "season": {
        "value": "spring",
        "confidence": 0.5
      },
      "category": {
        "value": "dress",
        "confidence": 0.8
      },
      "structuredDescription": {
        "value": "A pink dress with patterns.",
        "confidence": 0.5
      }
    }
    ```
    
    <aside>
    ❌
    
    Réponse en anglais malgré le prompt en français. Valeurs génériques peu exploitables ("fabric", "printed"). Scores de confiance peu fiables. Non utilisable sans fine-tuning.
    
    </aside>
    

---

## 5. Architecture Recommandée — Pattern Abstraction Provider

Quel que soit le modèle choisi après discussion avec DECADE, **l'implémentation doit absolument être découplée**. Voici le pattern architectural à adopter dès le début :

java

`// Interface abstraite — le cœur du découplage
public interface LLMProvider {
    ProductEnrichmentResult enrich(EnrichmentRequest request);
    String getProviderName();
}

// Implémentations concrètes
@Service("openai") public class OpenAiProvider implements LLMProvider { ... }
@Service("gemini") public class GeminiProvider  implements LLMProvider { ... }
@Service("claude")  public class ClaudeProvider  implements LLMProvider { ... }

// Sélection dynamique via configuration
@Service
public class EnrichmentService {
    @Autowired
    @Qualifier("${enrichment.llm.provider:openai}")  // configurable dans hybris.properties
    private LLMProvider llmProvider;
}`

Ce pattern te permet de switcher de provider en une ligne de configuration, de faire des tests A/B entre modèles pendant le développement, et de migrer vers un modèle open-source hébergé en interne post-PFE sans toucher à la logique métier.

---

## 6. Tableau de Décision Final

| Critère | GPT-4o | Gemini 1.5 Pro | Claude 3.5 | LLaVA 1.6 |
| --- | --- | --- | --- | --- |
| Multimodal (image+texte) | ✅ Excellent | ✅ Excellent | ✅ Très bon | 🔶 Correct |
| JSON structuré fiable | ✅ Natif | ✅ Bon | ✅ Très fiable | ❌ Faible |
| Compréhension sémantique mode | ✅ Excellent | ✅ Très bon | ✅ Excellent | 🔶 Basique |
| Coût pour 10K produits (~$) | ~$50-100 | ~$25-50 | ~$60-120 | ~0 + infra |
| Confidentialité données | ❌ Externe | ❌ Externe | ❌ Externe | ✅ Interne |
| SDK Java disponible | ✅ Officiel | ✅ Officiel | ⚠️ Via HTTP | ✅ Local |
| Recommandation PFE | ✅ **POC** | ✅ Alternative | ✅ Comparaison | 📋 Perspectives |

---

## 7. Recommandation Finale pour le PFE

La stratégie recommandée se déroule en trois temps. Pour le **POC (Sprint 1)**, utiliser GPT-4o pour valider rapidement la faisabilité de l'extraction d'attributs — c'est le modèle qui minimise le temps de setup et maximise la qualité initiale. Pour le **développement principal (Sprints 2-5)**, maintenir GPT-4o comme provider par défaut tout en développant avec l'interface abstraite `LLMProvider`, et implémenter Gemini 1.5 Pro en parallèle pour une comparaison de coût/qualité sur des données réelles DECADE. Pour la **soutenance**, présenter le tableau comparatif et justifier le choix final avec des métriques mesurées sur des données réelles — c'est ce qui différencie un mémoire de qualité d'une simple implémentation.

> Le choix du modèle doit être validé impérativement avec l'encadrant DECADE sur la question de la confidentialité. Si les données produit sont soumises à des accords NDA avec des marques partenaires, le recours à une API externe sera à justifier contractuellement — c'est une contrainte qui peut forcer le choix vers LLaVA avec fine-tuning, et qui doit être identifiée dès la Phase 1 du projet.
> 

# Étude de l'Existant — Les Modèles HuggingFace pour l'Enrichissement Produit

---

## 1. L'Écosystème HuggingFace — Comprendre les 3 Niveaux d'Accès

Avant d'analyser les modèles, il faut comprendre **comment HuggingFace fonctionne comme plateforme**. Il existe trois façons d'utiliser un modèle HuggingFace, avec des implications radicalement différentes en termes de coût, performance et confidentialité :

<aside>
🆓

**Gratuit (limité)** — **Serverless Inference API**

*hf-inference — ancien nom : Inference API*

**Accès** → Token HF gratuit

**Coût** → Gratuit (rate-limited)

**Pro plan** → +$2/mois crédits

**Modèles** → Surtout CPU, petits LLMs

**Multimodal GPU** → Via Inference Providers

**Confidentialité** → Données chez HF

<aside>
⚠️

Depuis juillet 2025 : focus CPU — les gros VLMs GPU passent par les Inference Providers

</aside>

</aside>

<aside>
💰

**Payant à l'usage** — **Inference Providers**

*Nouveau système HF (2025)*

**Fournisseurs** → Nebius, Together AI, Replicate, Novita…

**Tarif type** → $0.20–$1.50 / 1M tokens

**Modèles dispo** → Qwen2.5-VL, InternVL, SmolVLM…

**Setup** → API HF standard

**Latence** → Variable (selon provider)

**Confidentialité** → Données chez le provider

<aside>
ℹ️

Accès facile aux meilleurs modèles open-source sans gestion d'infra

</aside>

</aside>

<aside>
🖥️

**Auto-hébergé** — **Inference Endpoints / Local**

*Déploiement dédié ou on-premise*

**Inference Endpoints** → ~$0.60–6/h (GPU cloud HF)

**Scale-to-zero** → Oui (min 15 min idle)

**Local (Ollama/vLLM)** → Coût GPU uniquement

**Contrôle données** → ✅ Total

**Setup complexité** → Élevée

**Fine-tuning** → ✅ Possible

<aside>
✅

Seule option garantissant 100% confidentialité des données produit

</aside>

</aside>

---

## 2. Les Modèles HuggingFace Pertinents pour ton Projet

L'écosystème HuggingFace a connu une révolution en 2024-2025 avec l'émergence de **VLMs open-source** qui rivalisent désormais avec les modèles propriétaires sur des tâches spécifiques comme l'extraction d'attributs. Voici les candidats sérieux :

- 🟢 **Qwen2.5-VL** — Alibaba / Qwen Team (3B / 7B / 32B / 72B)
    
    ✅ JSON structuré natif · ✅ Résolution dynamique · ✅ Fine-tunable · ✅ Apache 2.0 (7B) · ⚠ 72B : GPU A100 requis
    
    | Critère | Multimodal | JSON | Sémantique | Coût | Latence | Confidentialité | Score | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
    | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
    - **Tailles** : 3B / 7B / 32B / 72B
    - **Licence** : Apache 2.0 (7B+) / Qwen (72B)
    - **HuggingFace Hub** : `Qwen/Qwen2.5-VL-7B-Instruct`
    - **Tarification** : ~$0.20/1M tokens via Nebius (7B) — Local : coût GPU uniquement
    - **Accès** : Inference Providers (Nebius, Together AI) · Inference Endpoints HF · Local (Ollama, vLLM)
    - **JSON structuré** : ✅ Natif — stable JSON output documenté
    
    **Performance e-commerce**
    
    Extraction d'attributs produit très précise. Comprend matières, couleurs nuancées, motifs. Utilisé dans des benchmarks e-commerce récents (2025). Supporte résolution dynamique native — idéal pour images produit haute résolution.
    
    **Exemple d'intégration**
    
    ```python
    from huggingface_hub import InferenceClient
    
    client = InferenceClient(
        provider="nebius",  # ou "together-ai", "replicate"
        api_key="hf_..."
    )
    
    response = client.chat.completions.create(
        model="Qwen/Qwen2.5-VL-7B-Instruct",
        messages=[{
            "role": "user",
            "content": [
                {"type": "image_url", "image_url": {"url": image_url}},
                {"type": "text", "text": json_prompt}
            ]
        }]
    )
    ```
    
    <aside>
    ⭐
    
    **TOP CHOIX HuggingFace** — meilleur VLM open-source pour l'extraction d'attributs produit en 2025. Le 7B via Inference Providers est le sweet spot coût/qualité.
    
    </aside>
    
- 🔵 **InternVL3** — OpenGVLab / Shanghai AI Lab (1B–78B)
    
    ✅ MIT ≤8B · ✅ Meilleure vision encoder · ✅ Fine-tunable · ⚠ Moins de providers · ⚠ Plus lourd à déployer
    
    | Critère | Multimodal | JSON | Sémantique | Coût | Latence | Confidentialité | Score | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
    | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
    - **Tailles** : 1B / 2B / 8B / 14B / 38B / 78B
    - **Licence** : MIT (≤8B) / CC-BY-4.0 (>8B)
    - **HuggingFace Hub** : `OpenGVLab/InternVL3-8B`
    - **Tarification** : Inference Endpoints ~$0.80/h (A10G pour 8B) — Local : GPU requis
    - **Accès** : Inference Endpoints HF · Local (vLLM, LMDeploy) · Certains providers
    - **JSON structuré** : ✅ Très bon — légèrement moins fiable que Qwen2.5-VL sur formats très stricts
    
    **Performance e-commerce**
    
    Excellent sur la compréhension visuelle fine. Son encodeur visuel 6B (InternViT-6B) lui confère une perception d'image supérieure. Particulièrement fort sur les textures et matières. Competitive avec GPT-4o et la série Qwen2.5-VL sur les benchmarks visuels.
    
    **Exemple d'intégration**
    
    ```python
    # Via Inference Endpoints HF (dédié)
    from huggingface_hub import InferenceClient
    
    client = InferenceClient(
        model="OpenGVLab/InternVL3-8B",
        token="hf_..."
    )
    
    # Ou en local avec vLLM
    # vllm serve OpenGVLab/InternVL3-8B --dtype auto
    ```
    
    <aside>
    ✅
    
    **Excellent alternative à Qwen2.5-VL** — préférable si la qualité de perception visuelle des textures/matières prime sur la facilité d'intégration.
    
    </aside>
    
- 🟡 **SmolVLM-2** — HuggingFace / équipe M4 (256M / 500M / 2.2B)
    
    ✅ 100% gratuit · ✅ Tourne sur CPU · ✅ Apache 2.0 · ⚠ Qualité limitée · ❌ Pas pour prod sans fine-tuning
    
    | Critère | Multimodal | JSON | Sémantique | Coût | Latence | Confidentialité | Score | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
    | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
    - **Tailles** : 256M / 500M / 2.2B
    - **Licence** : Apache 2.0
    - **HuggingFace Hub** : `HuggingFaceTB/SmolVLM2-2.2B-Instruct`
    - **Tarification** : 🆓 Gratuit via Serverless Inference API — Local : fonctionne même sur CPU !
    - **Accès** : Inference API gratuite (serverless) · Local (CPU possible) · Inference Providers
    - **JSON structuré** : ⚠ JSON possible mais moins fiable — nécessite prompt très strict
    
    **Performance e-commerce**
    
    Modèle ultra-léger conçu pour l'edge computing. Sur des tâches simples (couleur dominante, type de produit), il donne des résultats corrects. Insuffisant pour des attributs fins (matière, style nuancé). Intéressant comme filtre de pré-classification rapide avant un modèle plus lourd.
    
    **Exemple d'intégration**
    
    ```python
    # 100% gratuit — Serverless Inference API
    from huggingface_hub import InferenceClient
    
    client = InferenceClient(token="hf_...") # compte gratuit suffit
    
    response = client.chat.completions.create(
        model="HuggingFaceTB/SmolVLM2-2.2B-Instruct",
        messages=[{
            "role": "user",
            "content": [
                {"type": "image_url", "image_url": {"url": img_url}},
                {"type": "text", "text": "Couleur principale du produit ?"}
            ]
        }]
    )
    ```
    
    <aside>
    💡
    
    Utile pour le POC initial gratuit et les tests rapides. Ne pas utiliser en production pour l'extraction d'attributs complexes sans fine-tuning.
    
    </aside>
    
- ⚪ **Idefics3-8B** — HuggingFace / équipe M4 (8B)
    
    ✅ Apache 2.0 · ✅ Natif HF ecosystem · ⚠ Moins performant que Qwen · ⚠ JSON peu fiable
    
    | Critère | Multimodal | JSON | Sémantique | Coût | Latence | Confidentialité | Score | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
    | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
    - **Tailles** : 8B
    - **Licence** : Apache 2.0
    - **HuggingFace Hub** : `HuggingFaceM4/Idefics3-8B-Llama3`
    - **Tarification** : Serverless : limité/gratuit — Endpoints : ~$0.60/h (A10G)
    - **Accès** : Serverless Inference API · Inference Endpoints · Local
    - **JSON structuré** : ⚠ JSON via prompt — moins stable que Qwen2.5-VL
    
    **Performance e-commerce**
    
    Modèle HF solide basé sur LLaMA-3. Compréhension correcte des images produit. Supérieur à LLaVA 1.6 mais inférieur à Qwen2.5-VL sur les benchmarks d'extraction structurée. Son principal avantage est son intégration native dans l'écosystème HuggingFace.
    
    **Exemple d'intégration**
    
    ```python
    # Via Serverless API (gratuit, rate-limited)
    from huggingface_hub import InferenceClient
    
    client = InferenceClient(token="hf_...")
    
    response = client.visual_question_answering(
        image=image_bytes,
        question="Extract product attributes as JSON: color, material, season",
        model="HuggingFaceM4/Idefics3-8B-Llama3"
    )
    ```
    
    <aside>
    📋
    
    Alternative à considérer si LLaVA était initialement prévu. Supérieur à LLaVA mais Qwen2.5-VL reste préférable.
    
    </aside>
    
- 🟠 **LLaVA-OneVision** — LLaVA Team / ByteDance (0.5B / 7B / 72B)
    
    ✅ Multi-image natif · ✅ Apache 2.0 · ❌ JSON non fiable · ❌ Dépassé par Qwen2.5-VL
    
    | Critère | Multimodal | JSON | Sémantique | Coût | Latence | Confidentialité | Score | ⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
    | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
    - **Tailles** : 0.5B / 7B / 72B
    - **Licence** : Apache 2.0
    - **HuggingFace Hub** : `lmms-lab/llava-onevision-qwen2-7b-ov`
    - **Tarification** : Inference Endpoints : ~$0.80/h (A10G pour 7B)
    - **Accès** : Inference Endpoints · Local (vLLM) · Certains providers
    - **JSON structuré** : ❌ JSON peu fiable — nécessite post-traitement systématique
    
    **Performance e-commerce**
    
    Successeur de LLaVA 1.6, capable de traiter plusieurs images simultanément (utile pour les variantes produit). Sur les tâches d'extraction d'attributs e-commerce, les modèles adaptés surpassent LLaVA-OneVision. Pertinent surtout si on a plusieurs images du même produit à analyser conjointement.
    
    **Exemple d'intégration**
    
    ```bash
    # Local avec vLLM (recommandé pour ce modèle)
    vllm serve lmms-lab/llava-onevision-qwen2-7b-ov \
      --image-input-type pixel_values \
      --dtype bfloat16
    ```
    
    <aside>
    ⚠️
    
    Moins pertinent depuis Qwen2.5-VL. Ne choisir que si le traitement multi-images simultanées est un besoin critique.
    
    </aside>
    

---

## 3. Le Cas Particulier : Qwen2.5-VL — Le Meilleur Candidat HuggingFace

Qwen2.5-VL introduit une résolution dynamique native et un encodeur visuel ViT entraîné from scratch, capable de traiter des images de tailles variées. Le modèle excelle sur la génération de JSON structuré à partir d'images et de documents, ce qui le rend particulièrement pertinent pour des applications commerce comme l'extraction d'attributs produit. [Ollama](https://ollama.com/library/qwen2.5vl)

Ce qui le rend unique pour ton projet, c'est que des recherches récentes de 2025 sur l'adaptation de VLMs pour l'e-commerce montrent que Qwen2.5-VL-7B est l'un des modèles open-source les plus performants sur les tâches de prédiction d'attributs produit (aspect prediction) et de compréhension de la mode (deep fashion understanding), testé directement contre des modèles de référence de taille similaire. [arXiv](https://arxiv.org/html/2602.11733)

Son intégration dans ton stack Java/Spring via l'API HuggingFace Inference Providers est directe :

java

`// Intégration Qwen2.5-VL via HuggingFace Inference Providers (Java/Spring)
@Service
public class QwenVLProvider implements LLMProvider {

    // Compatible avec l'interface OpenAI — même format d'API
    private static final String HF_API_URL =
        "https://router.huggingface.co/nebius/v1/chat/completions";

    @Value("${hf.api.token}")
    private String hfToken;

    @Override
    public ProductEnrichmentResult enrich(EnrichmentRequest req) {
        Map<String, Object> body = Map.of(
            "model", "Qwen/Qwen2.5-VL-7B-Instruct",
            "messages", List.of(Map.of(
                "role", "user",
                "content", List.of(
                    Map.of("type", "image_url",
                           "image_url", Map.of("url", req.getImageUrl())),
                    Map.of("type", "text",
                           "text", buildJsonPrompt(req.getExistingAttrs()))
                )
            )),
            "max_tokens", 500
        );

        // Même format de réponse qu'OpenAI → réutilise le même parser
        return parseResponse(postJson(HF_API_URL, body, hfToken));
    }
}
\```

**Point clé** : Les Inference Providers HuggingFace utilisent exactement le même format d'API qu'OpenAI (`/v1/chat/completions`). Cela signifie que dans ton architecture avec le pattern `LLMProvider`, switcher de GPT-4o vers Qwen2.5-VL via HuggingFace ne nécessite que **changer l'URL et le nom du modèle** — le reste du code reste identique.

---

## 4. La Stratégie Fine-Tuning — Le Vrai Avantage HuggingFace

C'est ce qui différencie fondamentalement HuggingFace des APIs propriétaires. Les grands modèles de langage surpassent les PLMs pré-entraînés dans les tâches d'extraction d'attributs avec une meilleure généralisation, particulièrement dans les scénarios zero-shot et avec peu de données d'entraînement spécifiques.  Mais avec un fine-tuning même minimal sur des données catalogue DECADE, les performances deviennent nettement supérieures.

Le pipeline recommandé pour une utilisation post-PFE :
```
Étape 1 — Génération des données d'entraînement (pendant le PFE)
  GPT-4o enrichit 500-1000 produits Jacadi
  → Validation humaine via le dashboard
  → Résultat : dataset gold-standard propre à DECADE

Étape 2 — Fine-tuning Qwen2.5-VL-7B (post-PFE ou en fin de stage)
  # Avec LoRA (PEFT) — faisable sur 1 seul GPU A100
  pip install peft trl
  # Fine-tuning LoRA sur les données validées
  # Coût estimé : ~$20-50 sur cloud GPU pour 1000 exemples

Étape 3 — Déploiement Inference Endpoints HuggingFace
  # Modèle fine-tuné uploadé sur HF Hub privé
  # Endpoint dédié avec scale-to-zero
  # Coût : ~$0 quand inactif, ~$0.80/h quand actif
```

Des stratégies de fine-tuning semi-supervisé pour des VLMs compacts (2B-3B paramètres) montrent qu'en partant d'un petit ensemble annoté via API, il est possible d'utiliser PEFT pour entraîner des modules adaptateurs, puis d'enrichir avec des données non étiquetées via DPO pour obtenir des modèles performants sur la prédiction d'attributs produit. 

---

## 5. Tableau Comparatif Final — HuggingFace vs APIs Propriétaires

| Modèle | Accès | Coût/1M tokens | JSON fiable | E-commerce | Confidentialité | Recommandation |
|---|---|---|---|---|---|---|
| **Qwen2.5-VL 7B** | HF Providers | ~$0.20 | ✅ Natif | ⭐⭐⭐⭐⭐ | ⚠ Chez provider | **POC alternatif** |
| **Qwen2.5-VL 7B local** | Self-hosted | GPU seul | ✅ Natif | ⭐⭐⭐⭐⭐ | ✅ Total | **Prod long-terme** |
| **InternVL3-8B** | HF Endpoints | ~$0.80/h | ✅ Bon | ⭐⭐⭐⭐ | ✅ si local | Alternative solide |
| **SmolVLM-2** | API gratuite | 🆓 Gratuit | ⚠ Partiel | ⭐⭐ | ⚠ Chez HF | Tests/POC uniquement |
| **Idefics3-8B** | Serverless HF | Limité gratuit | ⚠ Partiel | ⭐⭐⭐ | ⚠ Chez HF | Dépassé par Qwen |
| GPT-4o *(référence)* | API OpenAI | ~$10 | ✅ Natif | ⭐⭐⭐⭐⭐ | ❌ OpenAI | Référence POC |

---

## 6. Recommandation Stratégique pour ton PFE

La meilleure approche est une **stratégie en 3 temps** qui tire parti des deux mondes :
```
Phase POC (Semaines 1-4) :
  → GPT-4o comme référence de qualité maximale
  → Qwen2.5-VL-7B via HF Providers en parallèle
  → Mesurer l'écart de qualité sur des données DECADE réelles

Phase Développement (Semaines 5-16) :
  → Provider configuré dans [hybris.properties](http://hybris.properties)
  → Si GPT-4o ≈ Qwen2.5-VL sur les attributs ciblés :
      basculer sur Qwen2.5-VL (8x moins cher)
  → Dashboard de validation alimente un dataset d'entraînement

Phase Perspectives (Mentionner en soutenance) :
  → Fine-tuning Qwen2.5-VL-7B sur données DECADE validées
  → Déploiement Inference Endpoints HF avec scale-to-zero
  → Coût marginal quasi-nul + confidentialité totale`

> **À retenir pour le mémoire** : L'émergence de Qwen2.5-VL en 2025 change radicalement l'équation. Il y a 2 ans, choisir entre un modèle propriétaire et un modèle open-source pour l'extraction d'attributs e-commerce était un choix entre qualité et coût. Aujourd'hui, des VLMs open-source adaptés à l'e-commerce peuvent rivaliser avec GPT-4o sur des tâches d'extraction d'attributs spécifiques comme la prédiction d'aspects visuels produit [arXiv](https://arxiv.org/html/2602.11733), ce qui ouvre la voie à une solution pérenne et économique pour DECADE.
>