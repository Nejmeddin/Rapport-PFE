# Tests avec Google AI Studio (Gratuut)

<aside>

#### gemini-2.5-flash: détection d’anomalies et enrichissement

```json
{
  "meta": {
    "stage": "google",
    "model": "gemini-2.5-flash",
    "confidenceThreshold": 0.7,
    "productFilter": [],
    "generatedAt": "2026-04-07T12:13:23.724544+00:00"
  },
  "products": [
    {
      "productId": "2045144_895",
      "productVersion": 95,
      "category": "clothing",
      "imagesUsed": [
        "C:\\Users\\NejmeddinBENMAATOUG\\Desktop\\PFE\\working on models to test\\resources\\images\\2045144_895\\1.jpg",
        "C:\\Users\\NejmeddinBENMAATOUG\\Desktop\\PFE\\working on models to test\\resources\\images\\2045144_895\\2.jpg",
        "C:\\Users\\NejmeddinBENMAATOUG\\Desktop\\PFE\\working on models to test\\resources\\images\\2045144_895\\3.jpg",
        "C:\\Users\\NejmeddinBENMAATOUG\\Desktop\\PFE\\working on models to test\\resources\\images\\2045144_895\\4.jpg"
      ],
      "categoryAttributesFile": "C:\\Users\\NejmeddinBENMAATOUG\\Desktop\\PFE\\working on models to test\\Ressources de Modele LLM\\attributs_par_categorie\\clothing_attributes.json",
      "initialAttributes": {
        "Description": {
          "displayName": "Description",
          "values": [
            "Ode à la douceur et aux belles journées, la blouse bébé fille en tissu Liberty transporte dans une ambiance florale opulente et poétique. Modernisée d'une collerette surdimensionnée et de manches volantées, elle se portera du printemps à l'été avec un pantalon ou un short.\n- Blouse en coton bébé fille\n- Manches volantées\n- Large collerette\n- Ouverture boutonnée au dos\n- Tissu Liberty Clarabell coloration exclusive Jacadi"
          ]
        },
        "category": {
          "displayName": "Catégorie",
          "values": [
            "Vêtements"
          ]
        },
        "product": {
          "displayName": "Produit",
          "values": [
            "Chemises, Blouses"
          ]
        },
        "product_type": {
          "displayName": "Type de produit",
          "values": [
            "Chemisiers"
          ]
        },
        "care_instructions": {
          "displayName": "Instructions d'entretien",
          "values": [
            "Repassage faible",
            "Pas de pressing",
            "Pas de sèche-linge",
            "Lavage a 30°C",
            "Chlore interdit"
          ]
        },
        "discount_percentage": {
          "displayName": "Promotion",
          "values": [
            "No Discount"
          ]
        },
        "size": {
          "displayName": "Taille",
          "values": [
            "06M",
            "12M",
            "18M",
            "24M",
            "36M"
          ]
        }
      },
      "analysis": {
        "contradictions": [],
        "missing_category_attributes": [
          {
            "attribute_name": "age",
            "suggested_values": [
              "Baby"
            ],
            "source": "description",
            "rationale": "The description explicitly states 'blouse bébé fille', indicating the target age group.",
            "evidence": "blouse bébé fille",
            "confidence": 1.0
          },
          {
            "attribute_name": "brand",
            "suggested_values": [
              "Jacadi"
            ],
            "source": "description",
            "rationale": "The description mentions 'coloration exclusive Jacadi', strongly implying Jacadi is the brand.",
            "evidence": "Tissu Liberty Clarabell coloration exclusive Jacadi",
            "confidence": 0.9
          },
          {
            "attribute_name": "closing",
            "suggested_values": [
              "Buttons"
            ],
            "source": "both",
            "rationale": "The description states 'Ouverture boutonnée au dos' (buttoned opening at the back), and the images confirm buttons on the back.",
            "evidence": "Ouverture boutonnée au dos (description), visible buttons on the back (image 4)",
            "confidence": 1.0
          },
          {
            "attribute_name": "collar_type",
            "suggested_values": [
              "Ruffled collar"
            ],
            "source": "both",
            "rationale": "The description mentions 'Large collerette' (large ruffled collar), which is clearly visible in the images.",
            "evidence": "Large collerette (description), visual observation of the collar (images)",
            "confidence": 1.0
          },
          {
            "attribute_name": "length",
            "suggested_values": [
              "Regular"
            ],
            "source": "image",
            "rationale": "Based on the product images, the blouse appears to be of a standard, regular length, not cropped or tunic-length.",
            "evidence": "Visual observation from images",
            "confidence": 0.8
          },
          {
            "attribute_name": "main_material",
            "suggested_values": [
              "Cotton"
            ],
            "source": "description",
            "rationale": "The description explicitly states 'Blouse en coton bébé fille' and 'Tissu Liberty', which is typically cotton.",
            "evidence": "Blouse en coton bébé fille",
            "confidence": 1.0
          },
          {
            "attribute_name": "occasions",
            "suggested_values": [
              "Casual"
            ],
            "source": "description",
            "rationale": "The description suggests it for 'belles journées' and 'du printemps à l'été', implying casual wear.",
            "evidence": "belles journées, du printemps à l'été",
            "confidence": 0.8
          },
          {
            "attribute_name": "pattern",
            "suggested_values": [
              "Floral"
            ],
            "source": "both",
            "rationale": "The images clearly display a floral pattern, and the description mentions 'ambiance florale'.",
            "evidence": "Visual observation from images, ambiance florale (description)",
            "confidence": 1.0
          },
          {
            "attribute_name": "product_color",
            "suggested_values": [
              "Pink",
              "Green",
              "White"
            ],
            "source": "image",
            "rationale": "The blouse features a multi-colored floral pattern with prominent pink, green, and white tones.",
            "evidence": "Visual observation from images",
            "confidence": 1.0
          },
          {
            "attribute_name": "product_features",
            "suggested_values": [
              "Ruffled sleeves",
              "Ruffled collar",
              "Button closure at back"
            ],
            "source": "both",
            "rationale": "These are distinct features explicitly mentioned in the description and clearly visible in the images.",
            "evidence": "Manches volantées, Large collerette, Ouverture boutonnée au dos (description), visual confirmation (images)",
            "confidence": 1.0
          },
          {
            "attribute_name": "seasons",
            "suggested_values": [
              "Spring",
              "Summer"
            ],
            "source": "description",
            "rationale": "The description explicitly states the blouse 'se portera du printemps à l'été'.",
            "evidence": "du printemps à l'été",
            "confidence": 1.0
          },
          {
            "attribute_name": "sex",
            "suggested_values": [
              "Girls"
            ],
            "source": "description",
            "rationale": "The description explicitly states 'blouse bébé fille'.",
            "evidence": "blouse bébé fille",
            "confidence": 1.0
          },
          {
            "attribute_name": "sleeve_length",
            "suggested_values": [
              "Short sleeve"
            ],
            "source": "both",
            "rationale": "The images clearly show short sleeves, and 'manches volantées' (ruffled sleeves) typically refers to short sleeves.",
            "evidence": "Visual observation from images, Manches volantées (description)",
            "confidence": 1.0
          },
          {
            "attribute_name": "sleeve_type",
            "suggested_values": [
              "Ruffled sleeve"
            ],
            "source": "both",
            "rationale": "The description explicitly mentions 'Manches volantées' and the images confirm the ruffled style of the sleeves.",
            "evidence": "Manches volantées (description), visual observation from images",
            "confidence": 1.0
          },
          {
            "attribute_name": "theme",
            "suggested_values": [
              "Floral",
              "Liberty"
            ],
            "source": "both",
            "rationale": "The product features a floral pattern ('ambiance florale') and is explicitly made from 'Tissu Liberty Clarabell'.",
            "evidence": "ambiance florale, Tissu Liberty Clarabell (description), visual pattern (images)",
            "confidence": 1.0
          }
        ],
        "new_attributes": [],
        "consistency_summary": "The product information is highly consistent between the images and the description. Existing attributes are accurate but could be significantly enriched with details explicitly present in the description and visible in the images, such as material, specific features, colors, and target demographic."
      },
      "meta": {
        "confidenceThreshold": 0.7,
        "model": "gemini-2.5-flash",
        "processedAt": "2026-04-07T12:13:23.724513+00:00"
      }
    }
  ]
}
```

#### gemma-4-31b-it: détection d’anomalies et enrichissement

```json
{
  "meta": {
    "stage": "google",
    "model": "gemma-4-31b-it",
    "confidenceThreshold": 0.7,
    "productFilter": [],
    "generatedAt": "2026-04-08T10:09:07.986165+00:00"
  },
  "products": [
    {
      "productId": "2045144_895",
      "productVersion": 95,
      "category": "clothing",
      "imagesUsed": [
        "C:\\Users\\NejmeddinBENMAATOUG\\Desktop\\PFE\\working on models to test\\resources\\images\\2045144_895\\1.jpg",
        "C:\\Users\\NejmeddinBENMAATOUG\\Desktop\\PFE\\working on models to test\\resources\\images\\2045144_895\\2.jpg",
        "C:\\Users\\NejmeddinBENMAATOUG\\Desktop\\PFE\\working on models to test\\resources\\images\\2045144_895\\3.jpg",
        "C:\\Users\\NejmeddinBENMAATOUG\\Desktop\\PFE\\working on models to test\\resources\\images\\2045144_895\\4.jpg"
      ],
      "categoryAttributesFile": "C:\\Users\\NejmeddinBENMAATOUG\\Desktop\\PFE\\working on models to test\\Ressources de Modele LLM\\attributs_par_categorie\\clothing_attributes.json",
      "initialAttributes": {
        "Description": {
          "displayName": "Description",
          "values": [
            "Ode à la douceur et aux belles journées, la blouse bébé fille en tissu Liberty transporte dans une ambiance florale opulente et poétique. Modernisée d'une collerette surdimensionnée et de manches volantées, elle se portera du printemps à l'été avec un pantalon ou un short.\n- Blouse en coton bébé fille\n- Manches volantées\n- Large collerette\n- Ouverture boutonnée au dos\n- Tissu Liberty Clarabell coloration exclusive Jacadi"
          ]
        },
        "category": {
          "displayName": "Catégorie",
          "values": [
            "Vêtements"
          ]
        },
        "product": {
          "displayName": "Produit",
          "values": [
            "Chemises, Blouses"
          ]
        },
        "product_type": {
          "displayName": "Type de produit",
          "values": [
            "Chemisiers"
          ]
        },
        "care_instructions": {
          "displayName": "Instructions d'entretien",
          "values": [
            "Repassage faible",
            "Pas de pressing",
            "Pas de sèche-linge",
            "Lavage a 30°C",
            "Chlore interdit"
          ]
        },
        "discount_percentage": {
          "displayName": "Promotion",
          "values": [
            "No Discount"
          ]
        },
        "size": {
          "displayName": "Taille",
          "values": [
            "06M",
            "12M",
            "18M",
            "24M",
            "36M"
          ]
        }
      },
      "analysis": {
        "contradictions": [
          {
            "type": "image_vs_attribute",
            "attribute_name": "description",
            "attribute_values_in_file": [
              "Ouverture boutonnée au dos"
            ],
            "observed_or_conflicting_value": "Ouverture boutonnée à l'avant",
            "why_contradiction": "The description explicitly states the button opening is at the back, but the images clearly show a center-front button placket.",
            "evidence": "Images 2, 3, and 4 show buttons running down the front of the garment.",
            "confidence": 1.0
          }
        ],
        "missing_category_attributes": [
          {
            "attribute_name": "closure",
            "suggested_values": [
              "Boutons"
            ],
            "source": "both",
            "rationale": "Both the description and images confirm the use of buttons for closure.",
            "evidence": "Description: 'Ouverture boutonnée'; Images: visible buttons on the front.",
            "confidence": 1.0
          },
          {
            "attribute_name": "collar_type",
            "suggested_values": [
              "Collerette"
            ],
            "source": "both",
            "rationale": "The description mentions a 'large collerette' and the images show a ruffled collar.",
            "evidence": "Description: '- Large collerette'; Images: visible ruffled collar.",
            "confidence": 1.0
          },
          {
            "attribute_name": "main_material",
            "suggested_values": [
              "Coton"
            ],
            "source": "description",
            "rationale": "The description explicitly mentions 'Blouse en coton'.",
            "evidence": "'- Blouse en coton bébé fille'",
            "confidence": 1.0
          },
          {
            "attribute_name": "pattern",
            "suggested_values": [
              "Floral"
            ],
            "source": "both",
            "rationale": "The description mentions 'ambiance florale' and 'tissu Liberty', and the images show a floral print.",
            "evidence": "Description: 'ambiance florale opulente et poétique'; Images: visible floral pattern.",
            "confidence": 1.0
          },
          {
            "attribute_name": "product_color",
            "suggested_values": [
              "Rose",
              "Vert",
              "Blanc"
            ],
            "source": "image",
            "rationale": "The garment features a white base with pink flowers and green leaves.",
            "evidence": "Visual observation of the product images.",
            "confidence": 0.95
          },
          {
            "attribute_name": "seasons",
            "suggested_values": [
              "Printemps",
              "Été"
            ],
            "source": "description",
            "rationale": "The description suggests wearing it from spring to summer.",
            "evidence": "Description: 'elle se portera du printemps à l'été'",
            "confidence": 1.0
          },
          {
            "attribute_name": "sex",
            "suggested_values": [
              "Fille"
            ],
            "source": "description",
            "rationale": "The description specifies it is for a baby girl.",
            "evidence": "'- Blouse en coton bébé fille'",
            "confidence": 1.0
          },
          {
            "attribute_name": "sleeve_length",
            "suggested_values": [
              "Courte"
            ],
            "source": "image",
            "rationale": "The images show short, ruffled sleeves.",
            "evidence": "Visual observation of the sleeve length in all images.",
            "confidence": 1.0
          },
          {
            "attribute_name": "sleeve_type",
            "suggested_values": [
              "Volantée"
            ],
            "source": "both",
            "rationale": "The description mentions 'manches volantées' and the images confirm the ruffled style.",
            "evidence": "Description: '- Manches volantées'; Images: visible ruffles on sleeves.",
            "confidence": 1.0
          }
        ],
        "new_attributes": [],
        "consistency_summary": "The product is a baby girl's floral cotton blouse. There is a critical contradiction between the description (which claims a back button opening) and the images (which show a front button opening). Several key category attributes such as material, pattern, color, and sex are present in the description or images but missing from the structured attributes."
      },
      "meta": {
        "confidenceThreshold": 0.7,
        "model": "gemma-4-31b-it",
        "processedAt": "2026-04-08T10:09:07.986142+00:00"
      }
    }
  ]
}
```

#### ficheProduct de test

```json
[
  {
    "productId": "2045144_895",
    "productVersion": 95,
    "attributes": {
      "media": {
        "displayName": {
          "fr-fr": "Média"
        },
        "attributesValues": [
          {
            "images": {
              "displayName": {
                "fr-fr": "Images"
              },
              "urls": [
                "images/2045144_895/1.jpg",
                "images/2045144_895/2.jpg",
                "images/2045144_895/3.jpg",
                "images/2045144_895/4.jpg"
              ]
            }
          }
        ]
      },
      "Description": {
        "displayName": {
          "fr-fr": "Description"
        },
        "attributesValues": [
          {
            "description_fr": {
              "displayName": {
                "fr-fr": "Description en français"
              },
              "value": "Ode à la douceur et aux belles journées, la blouse bébé fille en tissu Liberty transporte dans une ambiance florale opulente et poétique. Modernisée d'une collerette surdimensionnée et de manches volantées, elle se portera du printemps à l'été avec un pantalon ou un short.\n- Blouse en coton bébé fille\n- Manches volantées\n- Large collerette\n- Ouverture boutonnée au dos\n- Tissu Liberty Clarabell coloration exclusive Jacadi"
            }
          }
        ]
      },
      "category": {
        "displayName": {
          "fr-fr": "Catégorie"
        },
        "attributesValues": [
          {
            "clothing": {
              "displayName": {
                "fr-fr": "Vêtements"
              }
            }
          }
        ]
      },
      "product": {
        "displayName": {
          "fr-fr": "Produit"
        },
        "attributesValues": [
          {
            "shirts_blouses": {
              "displayName": {
                "fr-fr": "Chemises, Blouses"
              }
            }
          }
        ]
      },
      "product_type": {
        "displayName": {
          "fr-fr": "Type de produit"
        },
        "attributesValues": [
          {
            "blouses": {
              "displayName": {
                "fr-fr": "Chemisiers"
              }
            }
          }
        ]
      },
      
      "care_instructions": {
        "displayName": {
          "fr-fr": "Instructions d'entretien"
        },
        "attributesValues": [
          {
            "low_iron": {
              "displayName": {
                "fr-fr": "Repassage faible"
              }
            }
          },
          {
            "no_pressing": {
              "displayName": {
                "fr-fr": "Pas de pressing"
              }
            }
          },
          {
            "no_tumble_dry": {
              "displayName": {
                "fr-fr": "Pas de sèche-linge"
              }
            }
          },
          {
            "wash_at_30_degree_c": {
              "displayName": {
                "fr-fr": "Lavage a 30°C"
              }
            }
          },
          {
            "no_chlorine": {
              "displayName": {
                "fr-fr": "Chlore interdit"
              }
            }
          }
        ]
      },
      "discount_percentage": {
        "displayName": {
          "fr-fr": "Promotion"
        },
        "attributesValues": [
          {
            "no_discount": {
              "displayName": {
                "fr-fr": "No Discount"
              }
            }
          }
        ]
      },
      
      "size": {
        "displayName": {
          "fr-fr": "Taille"
        },
        "attributesValues": [
          {
            "zero_six_m": {
              "displayName": {
                "fr-fr": "06M"
              }
            }
          },
          {
            "twelve_m": {
              "displayName": {
                "fr-fr": "12M"
              }
            }
          },
          {
            "eighteen_m": {
              "displayName": {
                "fr-fr": "18M"
              }
            }
          },
          {
            "twenty_four_m": {
              "displayName": {
                "fr-fr": "24M"
              }
            }
          },
          {
            "thirty_six_m": {
              "displayName": {
                "fr-fr": "36M"
              }
            }
          }
        ]
      }
    }
  }
]

```

</aside>

# 📊 Rapport comparatif — Analyse produit par LLM

**Produit :** Blouse bébé fille en tissu Liberty — Jacadi
**Référence :** 2045144_895
**Date d'analyse :** 08 avril 2026
**Modèles comparés :** `gemini-2.5-flash` · `gemma-4-31b-it`**Référence humaine :** Claude (analyse basée sur les attributs initiaux + images uniquement)

> ⚠️ **Périmètre d'analyse :** Toutes les observations sont fondées exclusivement sur les **attributs initiaux fournis** (description, catégorie, produit, type, instructions d'entretien, promotion, tailles) et les **4 images du produit**. Aucune donnée externe (page web, prix, fiche technique) n'est prise en compte.
> 

---

## 1 — Détection des contradictions

| Attribut concerné | gemini-2.5-flash | gemma-4-31b-it | Observation Claude (images) |
| --- | --- | --- | --- |
| **Ouverture boutonnée** *(attribut description : "Ouverture boutonnée au dos")* | ✅ Aucune contradiction — description considérée correcte | ❌ Fausse contradiction — affirme que les boutons sont à l'avant (images 2, 3, 4) | ✅ Description confirmée — Image 2 : vue dos avec ligne d'ouverture centrale. Image 4 : bouton blanc clairement visible au dos. Aucun bouton visible à l'avant (images 1 et 3). |

> 💡 **Verdict :** gemma a confondu la vue dos (image 2) avec la vue avant. La valeur "Ouverture boutonnée au dos" dans les attributs initiaux est correcte et cohérente avec les images. gemini et Claude sont alignés : aucune contradiction réelle.
> 

---

## 2 — Attributs initiaux : évaluation de la cohérence image / attribut

| Attribut initial | Valeur fichier | gemini-2.5-flash | gemma-4-31b-it | Observation Claude |
| --- | --- | --- | --- | --- |
| **Catégorie** | Vêtements | ✅ Correct | ✅ Correct | ✅ Confirmé par les images — blouse = vêtement |
| **Produit** | Chemises, Blouses | ✅ Correct | ✅ Correct | ✅ Confirmé — blouse clairement visible sur toutes les images |
| **Type de produit** | Chemisiers | ✅ Correct | ✅ Correct | ✅ Confirmé |
| **Instructions d'entretien** | Repassage faible, Pas de pressing, Pas de sèche-linge, Lavage 30°C, Chlore interdit | ⬜ Non vérifiable sur image | ⬜ Non vérifiable sur image | ⬜ Non visible sur images — cohérent avec coton fin (mentionné dans la description) |
| **Promotion** | No Discount | ⬜ Non vérifiable sur image | ⬜ Non vérifiable sur image | ⬜ Non observable sur les images |
| **Taille** | 06M, 12M, 18M, 24M, 36M | ✅ Cohérent | ✅ Cohérent | ✅ Cohérent — proportions et gabarit bébé clairement visibles sur les images |

---

## 3 — Attributs manquants identifiés (comparaison exhaustive)

*Attributs présents dans la description ou visibles sur les images mais absents des attributs structurés initiaux.*

| Attribut | gemini-2.5-flash | gemma-4-31b-it | Observation Claude (images + description) |
| --- | --- | --- | --- |
| **age** | ✅ Baby | ❌ Non détecté | ✅ Baby — description : "blouse bébé fille". Proportions et gabarit bébé visibles sur les images. |
| **brand** | ✅ Jacadi | ❌ Non détecté | ✅ Jacadi — description : "coloration exclusive Jacadi". |
| **closing / closure** | ✅ Buttons | ✅ Boutons | ✅ Boutons au dos — description : "Ouverture boutonnée au dos". Confirmé image 4 : bouton blanc visible au dos. |
| **collar_type** | ✅ Ruffled collar | ✅ Collerette | ✅ Grande collerette volantée — description : "Large collerette". Images 1, 2, 3 : collerette surdimensionnée retombant sur les épaules. |
| **length** | ✅ Regular | ❌ Non détecté | ✅ Regular — image 1 : longueur standard, ni courte ni tunique. |
| **main_material** | ✅ Cotton | ✅ Coton | ✅ Coton — description : "Blouse en coton bébé fille" et "Tissu Liberty" (coton par nature). |
| **occasions** | ✅ Casual | ❌ Non détecté | ✅ Casual / habillé léger — description : "belles journées", "du printemps à l'été avec un pantalon ou un short". |
| **pattern** | ✅ Floral | ✅ Floral | ✅ Floral Liberty — description : "ambiance florale". Toutes les images confirment un motif floral dense et coloré. |
| **product_color** | ✅ Pink / Green / White | ✅ Rose / Vert / Blanc | ⚠️ Rose fuchsia, rose pâle, mauve, vert olive, blanc — les deux modèles omettent le **mauve/lilas** et le **rose pâle** pourtant bien présents sur les images. |
| **product_features** | ✅ Ruffled sleeves, Ruffled collar, Button closure at back | ❌ Non détecté | ✅ Manches volantées + collerette surdimensionnée + fermeture dos — description + images 1, 2, 3, 4 confirment les trois éléments. |
| **seasons** | ✅ Spring / Summer | ✅ Printemps / Été | ✅ Printemps / Été — description : "elle se portera du printemps à l'été". |
| **sex** | ✅ Girls | ✅ Fille | ✅ Fille — description : "blouse bébé fille". |
| **sleeve_length** | ✅ Short sleeve | ✅ Courte | ✅ Manches courtes — images 1 et 2 : manches n'atteignant pas le coude. |
| **sleeve_type** | ✅ Ruffled sleeve | ✅ Volantée | ✅ Manche volantée / papillon — description : "Manches volantées". Images 1 et 2 : manches évasées avec volant prononcé en forme d'aile. |
| **theme** | ✅ Floral / Liberty | ❌ Non détecté | ✅ Liberty / Floral — description : "Tissu Liberty Clarabell coloration exclusive Jacadi". |

---

## 4 — Nouveaux attributs générés

*Attributs qui n'existent pas dans le schéma initial mais qui pourraient être proposés à partir de la description et des images.*

| Attribut potentiel | gemini-2.5-flash | gemma-4-31b-it | Observation Claude |
| --- | --- | --- | --- |
| **Style de col (qualificatif)** | ⚠️ Inclus dans collar_type comme "Ruffled collar" | ⚠️ Inclus dans collar_type comme "Collerette" | ✅ La description précise "surdimensionnée" — qualificatif distinctif absent des deux modèles |
| **Nom du tissu** | ❌ Non proposé | ❌ Non proposé | ✅ "Liberty Clarabell" — mentionné explicitement dans la description, suffisamment précis pour être un attribut à part entière |
| **Style de manche (qualificatif)** | ⚠️ "Ruffled sleeve" sans précision | ⚠️ "Volantée" sans précision | ✅ Les images montrent une manche de type **papillon/aile** — plus précis que "volantée" seul |

> 💡 **Point faible commun :** Les deux modèles renvoient `new_attributes: []`. Aucun n'a proposé d'attributs réellement nouveaux, alors que la description contient des informations précises et distinctives (nom du tissu Liberty Clarabell, qualificatif "surdimensionnée") qui mériteraient d'être structurées.
> 

---

## 5 — Score global de performance

| Critère | gemini-2.5-flash | gemma-4-31b-it | Référence Claude |
| --- | --- | --- | --- |
| **Faux positifs (contradictions)** | ✅ 0 / 1 | ❌ 1 / 1 — erreur critique | ✅ 0 / 1 |
| **Attributs manquants trouvés** | ✅ 15 / 15 | ⚠️ 9 / 15 | ✅ 15 détectés |
| **Nouveaux attributs générés** | ❌ 0 | ❌ 0 | ⚠️ 3 proposés (dans périmètre description + images) |
| **Cohérence linguistique (FR)** | ⚠️ Valeurs en anglais | ✅ Valeurs en français | ✅ Français |
| **Précision des couleurs** | ⚠️ 3 couleurs (mauve et rose pâle absents) | ⚠️ 3 couleurs (mauve et rose pâle absents) | ✅ 5 nuances détectées sur les images |
| **Utilisation des images** | ✅ Bonne — cohérente avec la description | ❌ Mauvaise — confusion avant / dos | ✅ Analyse image par image |

---

## 6 — Conclusion

**gemini-2.5-flash est le modèle le plus fiable sur ce produit.**

- Il ne génère aucun faux positif sur la contradiction (erreur critique de gemma)
- Il est bien plus exhaustif sur les attributs manquants (15 vs 9)
- Sa principale faiblesse : les valeurs sont en anglais, inadapté à un contexte e-commerce francophone

**gemma-4-31b-it** répond en français, ce qui est un avantage pour Jacadi, mais sa confusion entre la vue avant et la vue dos du vêtement est une erreur critique pouvant introduire de fausses corrections dans un pipeline automatisé.

**Lacunes communes aux deux modèles (dans le périmètre images + description) :**

- Aucun ne génère de nouveaux attributs malgré des informations précises disponibles dans la description (nom du tissu, qualificatif "surdimensionnée")
- Les deux manquent le mauve/lilas et le rose pâle présents sur les images
- gemma manque des attributs importants : `age`, `brand`, `length`, `occasions`, `product_features`, `theme`

> 🔧 **Recommandation pipeline :** Pour améliorer la génération de nouveaux attributs, envisager de fournir aux modèles un schéma d'attributs cible plus explicite avec des exemples de valeurs attendues, afin de stimuler la proposition d'attributs au-delà du schéma existant.
> 

<aside>

#### test sur deuxieme produit

```json
[
  {
    "productId": "2043311_136",
    "productVersion": 1790,
    "attributes": {
      "media": {
        "displayName": {
          "fr-fr": "Média"
        },
        "attributesValues": [
          {
            "images": {
              "displayName": {
                "fr-fr": "Images"
              },
              "urls": [
                "images/2043311-136/1.jpg",
                "images/2043311-136/2.jpg",
                "images/2043311-136/3.jpg",
                "images/2043311-136/4.jpg"
              ]
            }
          }
        ]
      },
      "Description": {
        "displayName": {
          "fr-fr": "Description"
        },
        "attributesValues": [
          {
            "description_fr": {
              "displayName": {
                "fr-fr": "Description en français"
              },
              "value": "L'intemporelle robe chasuble bébé fille est déclinée en toile denim pour un look modernisé. Doublée en voile de coton doux et confortable, poches festonnées et pressions siglées aux épaules finalisent cette robe en jean à porter avec un body à collerette et un cardigan à motif cœur pour une silhouette chic et parisienne.\n- Robe chasuble bébé en jean\n- Poches plaquées festonnées\n- Doublure en voile de coton\n- Pressions aux épaules"
            }
          }
        ]
      },
      "category": {
        "displayName": {
          "fr-fr": "Catégorie"
        },
        "attributesValues": [
          {
            "clothing": {
              "displayName": {
                "fr-fr": "Vêtements"
              }
            }
          }
        ]
      },
      "product": {
        "displayName": {
          "fr-fr": "Produit"
        },
        "attributesValues": [
          {
            "dresses_skirts": {
              "displayName": {
                "fr-fr": "Robes, jupes"
              }
            }
          }
        ]
      },
      "product_type": {
        "displayName": {
          "fr-fr": "Type de produit"
        },
        "attributesValues": [
          {
            "dresses": {
              "displayName": {
                "fr-fr": "Robes"
              }
            }
          }
        ]
      },
      "age": {
        "displayName": {
          "fr-fr": "Univers"
        },
        "attributesValues": [
          {
            "newborn": {
              "displayName": {
                "fr-fr": "Naissance"
              }
            }
          },
          {
            "baby": {
              "displayName": {
                "fr-fr": "Bébé"
              }
            }
          }
        ]
      },
      "care_instructions": {
        "displayName": {
          "fr-fr": "Instructions d'entretien"
        },
        "attributesValues": [
          {
            "low_iron": {
              "displayName": {
                "fr-fr": "Repassage faible"
              }
            }
          },
          {
            "wash_at_30_degree_c": {
              "displayName": {
                "fr-fr": "Lavage a 30°C"
              }
            }
          },
          {
            "no_dry_cleaning": {
              "displayName": {
                "fr-fr": "Pas de nettoyage à sec"
              }
            }
          },
          {
            "no_chlorine": {
              "displayName": {
                "fr-fr": "Chlore interdit"
              }
            }
          },
          {
            "no_tumble_dry": {
              "displayName": {
                "fr-fr": "Pas de sèche-linge"
              }
            }
          }
        ]
      },
      "closure": {
        "displayName": {
          "fr-fr": "Fermeture"
        },
        "attributesValues": [
          {
            "snap": {
              "displayName": {
                "fr-fr": "Bouton pression"
              }
            }
          }
        ]
      },
      "discount_percentage": {
        "displayName": {
          "fr-fr": "Promotion"
        },
        "attributesValues": [
          {
            "fifty_percent": {
              "displayName": {
                "fr-fr": "50%"
              }
            }
          },
          {
            "no_discount": {
              "displayName": {
                "fr-fr": "No Discount"
              }
            }
          }
        ]
      },
      "finishes": {
        "displayName": {
          "fr-fr": "Finitions"
        },
        "attributesValues": [
          {
            "ruffles": {
              "displayName": {
                "fr-fr": "Manches à volants"
              }
            }
          },
          {
            "heart": {
              "displayName": {
                "fr-fr": "Coeur"
              }
            }
          },
          {
            "scallop_trim": {
              "displayName": {
                "fr-fr": "Garniture festonnée"
              }
            }
          },
          {
            "patch": {
              "displayName": {
                "fr-fr": "Patch"
              }
            }
          },
          {
            "decorative_snap": {
              "displayName": {
                "fr-fr": "Bouton-pression décoratif"
              }
            }
          }
        ]
      },
      "lining_material": {
        "displayName": {
          "fr-fr": "Materiau de la doublure"
        },
        "attributesValues": [
          {
            "cotton_voile": {
              "displayName": {
                "fr-fr": "Voile de coton"
              }
            }
          },
          {
            "cotton": {
              "displayName": {
                "fr-fr": "Coton"
              }
            }
          }
        ]
      },
      "main_material": {
        "displayName": {
          "fr-fr": "Matière"
        },
        "attributesValues": [
          {
            "denim": {
              "displayName": {
                "fr-fr": "Denim"
              }
            }
          },
          {
            "voile": {
              "displayName": {
                "fr-fr": "Voile"
              }
            }
          },
          {
            "cotton": {
              "displayName": {
                "fr-fr": "Coton"
              }
            }
          }
        ]
      },
      "pattern": {
        "displayName": {
          "fr-fr": "Motifs"
        },
        "attributesValues": [
          {
            "denim": {
              "displayName": {
                "fr-fr": "Denim"
              }
            }
          },
          {
            "solid": {
              "displayName": {
                "fr-fr": "Uni"
              }
            }
          }
        ]
      },
      "pocket_style": {
        "displayName": {
          "fr-fr": "Style de poche"
        },
        "attributesValues": [
          {
            "patch_pockets": {
              "displayName": {
                "fr-fr": "Poches plaquées"
              }
            }
          }
        ]
      },
      "product_color": {
        "displayName": {
          "fr-fr": "Couleur"
        },
        "attributesValues": [
          {
            "dark_denim": {
              "displayName": {
                "fr-fr": "Denim fonce"
              }
            }
          },
          {
            "blue": {
              "displayName": {
                "fr-fr": "Bleu"
              }
            }
          }
        ]
      },
      "sex": {
        "displayName": {
          "fr-fr": "Sexe"
        },
        "attributesValues": [
          {
            "girl": {
              "displayName": {
                "fr-fr": "Fille"
              }
            }
          }
        ]
      },
      "sleeve_type": {
        "displayName": {
          "fr-fr": "Type de manches"
        },
        "attributesValues": [
          {
            "sleeveless": {
              "displayName": {
                "fr-fr": "Sans manches"
              }
            }
          }
        ]
      },
      "style": {
        "displayName": {
          "fr-fr": "Style"
        },
        "attributesValues": [
          {
            "pinafore": {
              "displayName": {
                "fr-fr": "Robe chasuble"
              }
            }
          }
        ]
      },
      "size": {
        "displayName": {
          "fr-fr": "Taille"
        },
        "attributesValues": [
          {
            "zero_six_m": {
              "displayName": {
                "fr-fr": "06M"
              }
            }
          },
          {
            "twelve_m": {
              "displayName": {
                "fr-fr": "12M"
              }
            }
          },
          {
            "zero_three_m": {
              "displayName": {
                "fr-fr": "03M"
              }
            }
          },
          {
            "eighteen_m": {
              "displayName": {
                "fr-fr": "18M"
              }
            }
          }
        ]
      },
      "specificity_baby_dress": {
        "displayName": {
          "fr-fr": "Spécificité robé bébé"
        },
        "attributesValues": [
          {
            "without_bloomers": {
              "displayName": {
                "fr-fr": "Sans bloomers"
              }
            }
          }
        ]
      },
      "length": {
        "displayName": {
          "fr-fr": "Longueur"
        },
        "attributesValues": [
          {
            "knee": {
              "displayName": {
                "fr-fr": "Genou"
              }
            }
          },
          {
            "mini": {
              "displayName": {
                "fr-fr": "Mini"
              }
            }
          }
        ]
      },
      "occasions": {
        "displayName": {
          "fr-fr": "Occasion"
        },
        "attributesValues": [
          {
            "casual": {
              "displayName": {
                "fr-fr": "Décontracté"
              }
            }
          },
          {
            "classic": {
              "displayName": {
                "fr-fr": "Classique"
              }
            }
          }
        ]
      },
      "hemline": {
        "displayName": {
          "fr-fr": "Ourlet"
        },
        "attributesValues": [
          {
            "straight": {
              "displayName": {
                "fr-fr": "Droit"
              }
            }
          }
        ]
      },
      "brand": {
        "displayName": {
          "fr-fr": "Marque"
        },
        "attributesValues": [
          {
            "jacadi": {
              "displayName": {
                "fr-fr": "Jacadi"
              }
            }
          }
        ]
      },
      "secondary_material": {
        "displayName": {
          "fr-fr": "Materiau secondaire"
        },
        "attributesValues": [
          {
            "cotton": {
              "displayName": {
                "fr-fr": "Coton"
              }
            }
          },
          {
            "elasthane": {
              "displayName": {
                "fr-fr": "Élasthanne"
              }
            }
          }
        ]
      },
      "fit": {
        "displayName": {
          "fr-fr": "Coupe"
        },
        "attributesValues": [
          {
            "regular": {
              "displayName": {
                "fr-fr": "Coupe droite"
              }
            }
          }
        ]
      }
    }
  }
]

```

#### resultat

```json
{
  "meta": {
    "stage": "google",
    "model": "gemini-2.5-flash",
    "confidenceThreshold": 0.7,
    "productFilter": [],
    "generatedAt": "2026-04-10T11:22:48.705650+00:00"
  },
  "products": [
    {
      "productId": "2043311_136",
      "productVersion": 1790,
      "category": "clothing",
      "imagesUsed": [
        "C:\\Users\\NejmeddinBENMAATOUG\\Desktop\\PFE\\working on models to test\\resources\\images\\2043311-136\\1.jpg",
        "C:\\Users\\NejmeddinBENMAATOUG\\Desktop\\PFE\\working on models to test\\resources\\images\\2043311-136\\2.jpg",
        "C:\\Users\\NejmeddinBENMAATOUG\\Desktop\\PFE\\working on models to test\\resources\\images\\2043311-136\\3.jpg",
        "C:\\Users\\NejmeddinBENMAATOUG\\Desktop\\PFE\\working on models to test\\resources\\images\\2043311-136\\4.jpg"
      ],
      "categoryAttributesFile": "C:\\Users\\NejmeddinBENMAATOUG\\Desktop\\PFE\\working on models to test\\Ressources de Modele LLM\\attributs_par_categorie\\clothing_attributes.json",
      "initialAttributes": {
        "Description": {
          "displayName": "Description",
          "values": [
            "L'intemporelle robe chasuble bébé fille est déclinée en toile denim pour un look modernisé. Doublée en voile de coton doux et confortable, poches festonnées et pressions siglées aux épaules finalisent cette robe en jean à porter avec un body à collerette et un cardigan à motif cœur pour une silhouette chic et parisienne.\n- Robe chasuble bébé en jean\n- Poches plaquées festonnées\n- Doublure en voile de coton\n- Pressions aux épaules"
          ]
        },
        "category": {
          "displayName": "Catégorie",
          "values": [
            "Vêtements"
          ]
        },
        "product": {
          "displayName": "Produit",
          "values": [
            "Robes, jupes"
          ]
        },
        "product_type": {
          "displayName": "Type de produit",
          "values": [
            "Robes"
          ]
        },
        "age": {
          "displayName": "Univers",
          "values": [
            "Naissance",
            "Bébé"
          ]
        },
        "care_instructions": {
          "displayName": "Instructions d'entretien",
          "values": [
            "Repassage faible",
            "Lavage a 30°C",
            "Pas de nettoyage à sec",
            "Chlore interdit",
            "Pas de sèche-linge"
          ]
        },
        "closure": {
          "displayName": "Fermeture",
          "values": [
            "Bouton pression"
          ]
        },
        "discount_percentage": {
          "displayName": "Promotion",
          "values": [
            "50%",
            "No Discount"
          ]
        },
        "finishes": {
          "displayName": "Finitions",
          "values": [
            "Manches à volants",
            "Coeur",
            "Garniture festonnée",
            "Patch",
            "Bouton-pression décoratif"
          ]
        },
        "lining_material": {
          "displayName": "Materiau de la doublure",
          "values": [
            "Voile de coton",
            "Coton"
          ]
        },
        "main_material": {
          "displayName": "Matière",
          "values": [
            "Denim",
            "Voile",
            "Coton"
          ]
        },
        "pattern": {
          "displayName": "Motifs",
          "values": [
            "Denim",
            "Uni"
          ]
        },
        "pocket_style": {
          "displayName": "Style de poche",
          "values": [
            "Poches plaquées"
          ]
        },
        "product_color": {
          "displayName": "Couleur",
          "values": [
            "Denim fonce",
            "Bleu"
          ]
        },
        "sex": {
          "displayName": "Sexe",
          "values": [
            "Fille"
          ]
        },
        "sleeve_type": {
          "displayName": "Type de manches",
          "values": [
            "Sans manches"
          ]
        },
        "style": {
          "displayName": "Style",
          "values": [
            "Robe chasuble"
          ]
        },
        "size": {
          "displayName": "Taille",
          "values": [
            "06M",
            "12M",
            "03M",
            "18M"
          ]
        },
        "specificity_baby_dress": {
          "displayName": "Spécificité robé bébé",
          "values": [
            "Sans bloomers"
          ]
        },
        "length": {
          "displayName": "Longueur",
          "values": [
            "Genou",
            "Mini"
          ]
        },
        "occasions": {
          "displayName": "Occasion",
          "values": [
            "Décontracté",
            "Classique"
          ]
        },
        "hemline": {
          "displayName": "Ourlet",
          "values": [
            "Droit"
          ]
        },
        "brand": {
          "displayName": "Marque",
          "values": [
            "Jacadi"
          ]
        },
        "secondary_material": {
          "displayName": "Materiau secondaire",
          "values": [
            "Coton",
            "Élasthanne"
          ]
        },
        "fit": {
          "displayName": "Coupe",
          "values": [
            "Coupe droite"
          ]
        }
      },
      "analysis": {
        "contradictions": [
          {
            "type": "image_vs_attribute",
            "attribute_name": "finishes",
            "attribute_values_in_file": [
              "Manches à volants",
              "Coeur",
              "Garniture festonnée",
              "Patch",
              "Bouton-pression décoratif"
            ],
            "observed_or_conflicting_value": "Manches à volants",
            "why_contradiction": "The dress is sleeveless, as confirmed by the images and the 'sleeve_type' attribute ('Sans manches'). Therefore, it cannot have ruffled sleeves ('Manches à volants').",
            "evidence": "Image, existing attribute 'sleeve_type': ['Sans manches']",
            "confidence": 1.0
          },
          {
            "type": "image_vs_attribute",
            "attribute_name": "finishes",
            "attribute_values_in_file": [
              "Manches à volants",
              "Coeur",
              "Garniture festonnée",
              "Patch",
              "Bouton-pression décoratif"
            ],
            "observed_or_conflicting_value": "Coeur",
            "why_contradiction": "No heart motif is visible on the dress in the provided images or mentioned in the description as a feature of the dress itself. The description refers to a 'cardigan à motif cœur' as a styling suggestion, not a feature of the product.",
            "evidence": "Image, description",
            "confidence": 1.0
          },
          {
            "type": "image_vs_attribute",
            "attribute_name": "finishes",
            "attribute_values_in_file": [
              "Manches à volants",
              "Coeur",
              "Garniture festonnée",
              "Patch",
              "Bouton-pression décoratif"
            ],
            "observed_or_conflicting_value": "Patch",
            "why_contradiction": "No patch is visible on the dress in the provided images or mentioned in the description.",
            "evidence": "Image, description",
            "confidence": 1.0
          },
          {
            "type": "description_vs_attribute",
            "attribute_name": "secondary_material",
            "attribute_values_in_file": [
              "Coton",
              "Élasthanne"
            ],
            "observed_or_conflicting_value": "Élasthanne",
            "why_contradiction": "The description explicitly mentions 'toile denim' and 'voile de coton' but does not state 'Élasthanne' as a material. As per strict QA, facts not explicitly present in the description or visible in images should not be assumed.",
            "evidence": "Description",
            "confidence": 0.9
          },
          {
            "type": "image_vs_attribute",
            "attribute_name": "length",
            "attribute_values_in_file": [
              "Genou",
              "Mini"
            ],
            "observed_or_conflicting_value": "Genou",
            "why_contradiction": "Based on the product images, the dress appears to be significantly above the knee, which is consistent with a 'Mini' length for a baby dress. 'Genou' (knee-length) seems inaccurate for the observed length.",
            "evidence": "Image",
            "confidence": 0.9
          },
          {
            "type": "attribute_vs_attribute",
            "attribute_name": "main_material",
            "attribute_values_in_file": [
              "Denim",
              "Voile",
              "Coton"
            ],
            "observed_or_conflicting_value": "Voile, Coton",
            "why_contradiction": "The description states the dress is made of 'toile denim' and is 'Doublée en voile de coton'. 'Voile' and 'Coton' are explicitly described as lining materials (also present in 'lining_material' attribute), not main materials. Listing them under 'main_material' is misleading.",
            "evidence": "Description: 'toile denim', 'Doublée en voile de coton'. Existing attribute 'lining_material': ['Voile de coton', 'Coton']",
            "confidence": 1.0
          }
        ],
        "missing_category_attributes": [
          {
            "attribute_name": "product_features",
            "suggested_values": [
              "Pressions siglées"
            ],
            "source": "description",
            "rationale": "The description explicitly highlights 'pressions siglées aux épaules' (branded snap buttons on the shoulders) as a specific detail of the product.",
            "evidence": "Description: 'pressions siglées aux épaules'",
            "confidence": 1.0
          },
          {
            "attribute_name": "seasons",
            "suggested_values": [
              "Toutes saisons",
              "Printemps/Automne"
            ],
            "source": "description",
            "rationale": "The description uses the term 'intemporelle' (timeless) and suggests layering with a body and cardigan, implying suitability for multiple seasons. Denim is also a versatile material.",
            "evidence": "Description: 'L'intemporelle robe chasuble bébé fille...', 'à porter avec un body à collerette et un cardigan...'",
            "confidence": 0.9
          }
        ],
        "new_attributes": [],
        "consistency_summary": "The product attributes show several inconsistencies. The 'finishes' attribute contains values ('Manches à volants', 'Coeur', 'Patch') not present on the product. 'Élasthanne' in 'secondary_material' is not supported by the description. 'Voile' and 'Coton' are incorrectly listed as 'main_material' when they are lining materials. The 'length' attribute could be more precise. Opportunities for enrichment include adding 'seasons' and 'Pressions siglées' to 'product_features'."
      },
      "meta": {
        "confidenceThreshold": 0.7,
        "model": "gemini-2.5-flash",
        "processedAt": "2026-04-10T11:22:48.705633+00:00"
      }
    }
  ]
}
```

</aside>